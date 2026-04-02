# signal-crawler

## data flow
```
scheduler
  ↓
queue (Redis)
  ↓
crawler worker
  ↓
pipeline engine
  ↓
storage (S3 / parquet)
  ↓
miner (serve)
```

## architecture
```
signal-crawler/
│
├── app/
│   ├── config.py
│
│   ├── core/                         # pure infra (절대 비즈니스 금지)
│   │   ├── logger.py
│   │   ├── utils.py
│   │   ├── hashing.py
│   │   └── time.py
│
│   ├── domain/                       # 🔥 모든 데이터 정의
│   │   ├── document.py
│   │   ├── keyword.py
│   │   ├── task.py
│   │   └── enums.py
│
│   ├── queue/                        # Redis queue system
│   │   ├── client.py
│   │   ├── producer.py
│   │   ├── consumer.py
│   │   ├── schemas.py                # task payload
│   │   └── dedup.py
│
│   ├── crawler/                      # 수집 시스템
│   │   ├── base.py
│   │   ├── registry.py
│   │   ├── fetcher.py                # HTTP layer
│   │   │
│   │   ├── middlewares/              # 🔥 안정성 핵심
│   │   │   ├── retry.py
│   │   │   ├── proxy.py
│   │   │   ├── rate_limit.py
│   │   │   └── circuit_breaker.py
│   │   │
│   │   └── sources/
│   │       ├── reddit.py
│   │       ├── x.py
│   │       └── web.py
│
│   ├── pipeline/                     # 🔥 데이터 처리 엔진
│   │   ├── engine.py                 # orchestration
│   │   ├── context.py                # 🔥 상태/로그/trace
│   │   ├── schema.py                 # 데이터 흐름 정의
│   │   │
│   │   └── stages/
│   │       ├── parse.py
│   │       ├── clean.py
│   │       ├── normalize.py
│   │       ├── enrich.py             # 🔥 중요 (LLM/추가 정보)
│   │       └── validate.py
│
│   ├── keyword/                      # 🔥 수익 엔진
│   │   ├── orchestrator.py
│   │   │
│   │   ├── strategies/
│   │   │   ├── extract.py
│   │   │   ├── expand.py
│   │   │   ├── trending.py
│   │   │   └── semantic.py
│   │   │
│   │   ├── ranking/                  # 🔥 핵심
│   │   │   ├── features.py
│   │   │   └── scorer.py
│   │   │
│   │   └── generator.py
│
│   ├── storage/                      # 저장 계층
│   │   ├── repository.py
│   │   │
│   │   ├── serializers/              # 🔥 포맷 분리
│   │   │   ├── json.py
│   │   │   └── parquet.py
│   │   │
│   │   └── backends/
│   │       ├── redis.py
│   │       ├── sqlite.py
│   │       ├── parquet.py
│   │       └── s3.py
│
│   ├── miner/                        # bittensor 인터페이스
│   │   ├── client.py
│   │   ├── protocol.py
│   │   ├── adapter.py
│   │   └── scorer.py                 # 🔥 validator 대응
│
│   ├── services/                     # orchestration only
│   │   ├── scheduler.py              # keyword → task 생성
│   │   ├── crawl_service.py          # task → crawler 실행
│   │   └── ingest_service.py         # pipeline → storage
│
│   ├── api/                          # validator / 외부 대응
│   │   ├── server.py
│   │   ├── routes.py
│   │   └── schemas.py
│
│   └── observability/                # 🔥 운영 필수
│       ├── metrics.py
│       ├── tracing.py
│       ├── logging.py
│       └── events.py                 # 🔥 수익 추적
│
├── workers/                          # 실행 단위 (scale-out)
│   ├── scheduler_worker.py
│   ├── crawler_worker.py
│   └── ingestion_worker.py
│
├── scripts/
│   ├── run_scheduler.py
│   ├── run_crawler.py
│   ├── run_ingestion.py
│   └── run_api.py
│
├── infra/
│   ├── redis/
│   │   └── redis.conf
│   └── env/
│       └── .env
│
├── docker/
│   ├── scheduler.Dockerfile
│   ├── crawler.Dockerfile
│   ├── ingestion.Dockerfile
│   ├── api.Dockerfile
│   └── miner.Dockerfile
│
├── data-universe/
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```
