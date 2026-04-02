# signal-crawler
```
signal-crawler/
│
├── app/
│   ├── config.py
│
│   ├── core/                        # 절대 비즈니스 로직 금지
│   │   ├── logger.py
│   │   ├── utils.py
│   │   ├── hashing.py
│   │   └── types.py
│
│   ├── domain/                      # 핵심: 데이터 모델 정의
│   │   ├── document.py              # raw → processed 구조
│   │   ├── keyword.py
│   │   └── task.py                  # queue payload schema
│
│   ├── queue/                       # Redis abstraction
│   │   ├── client.py
│   │   ├── producer.py
│   │   ├── consumer.py
│   │   └── dedup.py
│
│   ├── crawler/                     # 데이터 수집
│   │   ├── base.py
│   │   ├── registry.py              # crawler 선택 로직
│   │   ├── sources/
│   │   │   ├── reddit.py
│   │   │   ├── x.py
│   │   │   └── web.py               # playwright/crawl4ai
│   │   └── fetcher.py               # http / retry / rate limit
│
│   ├── pipeline/                    # 실행 엔진
│   │   ├── engine.py                # pipeline orchestration
│   │   ├── schema.py                # 데이터 흐름 정의
│   │   └── stages/
│   │       ├── parse.py
│   │       ├── clean.py
│   │       ├── normalize.py
│   │       └── validate.py
│
│   ├── keyword/                     # 수익 핵심
│   │   ├── orchestrator.py
│   │   ├── strategies/
│   │   │   ├── extract.py
│   │   │   ├── expand.py
│   │   │   ├── trending.py
│   │   │   └── semantic.py
│   │   ├── scorer.py
│   │   └── generator.py
│
│   ├── storage/                     # 저장 추상화
│   │   ├── repository.py            # 공통 인터페이스
│   │   └── backends/
│   │       ├── redis.py
│   │       ├── sqlite.py
│   │       ├── parquet.py
│   │       └── s3.py                # 필수 (data-universe 대비)
│
│   ├── miner/                       # bittensor 연결
│   │   ├── client.py
│   │   ├── protocol.py              # 요청/응답 정의
│   │   └── adapter.py               # 내부 → miner 포맷 변환
│
│   ├── services/                    # orchestration layer
│   │   ├── scheduler.py             # keyword → queue
│   │   ├── crawl_service.py         # queue → crawler
│   │   └── ingest_service.py        # pipeline → storage
│
│   ├── api/                         # validator 대응
│   │   ├── server.py
│   │   ├── routes.py
│   │   └── schemas.py
│
│   └── observability/               # 운영 핵심
│       ├── metrics.py
│       ├── tracing.py
│       └── logging.py
│
├── workers/                         # 실행 단위 분리 (중요)
│   ├── scheduler.py
│   ├── crawler.py
│   └── ingestion.py
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
├── data-universe/                   # 그대로 유지
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```
