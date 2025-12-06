# 🚀 Real-time CDC Data Pipeline Project

## 📖 프로젝트 개요
온프레미스 환경의 RDB 데이터를 클라우드 기반 데이터 레이크(S3)로 실시간 적재하기 위한 파이프라인을 구축한 토이 프로젝트입니다.
**MySQL**의 변경 사항(CDC)을 **Debezium**으로 캡처하여 **Kafka**로 전송하고, **Python**을 활용해 **Parquet** 포맷으로 변환 후 **MinIO(S3)**에 적재하는 End-to-End 과정을 구현했습니다.

## 🛠 사용 기술 (Tech Stack)
- **Source:** MySQL 8.0
- **CDC:** Debezium (Kafka Connect)
- **Message Queue:** Apache Kafka, Zookeeper
- **Processing:** Python (KafkaConsumer, Pandas, PyArrow)
- **Storage:** MinIO (S3 Compatible Object Storage)
- **Environment:** Docker, Docker Compose

## 🏗 아키텍처 (Architecture)
[MySQL] -> (Binlog) -> [Debezium] -> [Kafka] -> [Python Worker] -> (Parquet) -> [MinIO/S3]

## 🔥 주요 구현 내용
1. **Docker Compose 기반 인프라 구축**
   - Kafka, Zookeeper, MySQL, Kafka Connect, MinIO 컨테이너 구성
2. **CDC 파이프라인 구성**
   - Debezium Connector를 활용한 실시간 변경분 캡처 (Insert/Update/Delete)
   - MySQL 8.0 인증 문제(Public Key Retrieval) 트러블 슈팅 해결
3. **Stream to Batch 적재 구현**
   - Python을 이용해 Kafka 스트림 데이터를 Micro-batch 형태로 버퍼링
   - 데이터 레이크 표준 포맷인 **Parquet**로 변환 및 Snappy 압축 적용
   - `boto3`를 활용하여 S3 호환 스토리지(MinIO)에 적재

## 🚀 실행 방법 (How to run)
1. 인프라 실행: `docker-compose up -d`
2. 커넥터 등록: `curl -X POST ...` (connector.json 참고)
3. 워커 실행: `python kafka_to_s3.py`