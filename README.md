# signal-crawler
```
stream-crawler/
│
├── app/                         # Python main package
│   ├── __init__.py
│   ├── config.py
│
│   ├── core/                    # 공통 (절대 비대해지면 안됨)
│   │   ├── logger.py
│   │   ├── utils.py
│   │   └── hashing.py
│
│   ├── queue/                   # Redis abstraction
│   │   ├── redis_client.py
│   │   ├── task_queue.py
│   │   └── dedup.py
│
│   ├── crawler/                 # 데이터 수집
│   │   ├── base.py
│   │   ├── reddit.py
│   │   ├── x.py
│   │   └── fallback.py          # playwright / crawl4ai
│
│   ├── pipeline/                # 데이터 처리 (핵심 레이어)
│   │   ├── parser.py
│   │   ├── cleaner.py
│   │   ├── normalizer.py
│   │   └── validator.py         # 내부 품질 체크
│
│   ├── keyword/                 # 수익 엔진
│   │   ├── extractor.py
│   │   ├── expander.py
│   │   ├── scorer.py
│   │   └── generator.py
│
│   ├── storage/                 # 저장 레이어
│   │   ├── redis_cache.py
│   │   ├── sqlite.py
│   │   └── parquet.py
│
│   ├── services/                # 실행 단위 (중요)
│   │   ├── scheduler.py         # 키워드 생성 → queue push
│   │   ├── crawler_worker.py    # queue → 크롤링
│   │   └── ingestion_worker.py  # pipeline → 저장
│
│   ├── api/                     # validator 대응
│   │   ├── server.py
│   │   └── routes.py
│
│   └── miner/                   # data-universe bridge
│       └── client.py            # API 호출 wrapper
│
├── scripts/                     # 실행 스크립트
│   ├── run_scheduler.py
│   ├── run_worker.py
│   ├── run_api.py
│   └── run_all.sh
│
├── infra/                       # 인프라 설정
│   ├── redis/
│   │   └── redis.conf
│   └── env/
│       └── .env
│
├── docker/
│   ├── crawler.Dockerfile
│   ├── worker.Dockerfile
│   ├── api.Dockerfile
│   └── miner.Dockerfile
│
├── data-universe/               # (git clone) ← 그대로 유지
│
├── docker-compose.yml
├── requirements.txt
└── README.md
```
