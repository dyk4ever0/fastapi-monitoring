#fastapi 모니터링 구성
##프로젝트 구조
```
home/ubuntu/
├── llm_AnswerGen-sroberta_hybrid/    # API 애플리케이션
│   ├── venv/
│   ├── app.py                        # FastAPI (5000번 포트)
│   └── utils/utils_logs/*.log        # 로그 파일들
│
└── monitoring/                       # 모니터링 스택
    ├── docker-compose.yml
    └── config/
        ├── prometheus/               # 메트릭 수집
        ├── grafana/                  # 시각화
        └── loki/                     # 로그 수집
```

##구성 요소
- FastAPI: 5000번 포트에서 실행 (nohup uvicorn app:app --host 0.0.0.0 --port 5000)
- 모니터링 스택 (도커 컨테이너):
    - Prometheus (9090): 메트릭 수집
    - Grafana (3000): 대시보드 시각화
    - Loki (3100): 로그 저장/관리
    - Promtail (9080): 로그 수집
    - Node-exporter (9100): 시스템 메트릭

## 데이터 흐름
1. 메트릭 파이프라인:
    - FastAPI /metrics → Prometheus → Grafana
2. 로그 파이프라인: 
    - 애플리케이션 로그 → Promtail → Loki → S3 → Grafana

## 실행 방법
### 1. API 실행
```
cd llm_AnswerGen-sroberta_hybrid
source venv/bin/activate
nohup uvicorn app:app --host 0.0.0.0 --port 5000
```

### 2. 모니터링 실행
```
cd ../monitoring
docker compose up -d
```

## 접속 정보
- API: http://host:5000
- Grafana: http://host:3000
- Prometheus: http://host:9090