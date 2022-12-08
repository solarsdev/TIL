# Databases

## 데이터베이스 선택 기준

- AWS에서 제공하는 다양한 데이터베이스 중 어떤것을 선택해야 하는가
- 데이터베이스의 선택 기준
  - 읽기 빈도, 처리량, 이러한 기준의 도중 변경 유무
  - 데이터의 저장 용량, 어느정도로 늘어나는지
  - 데이터의 가용성, 데이터 무결성
  - 지연속도 및 동시접속수
  - 데이터의 모델, 관계형 또는 비관계형
  - 스키마 설계
  - 라이센스 및 비용
  - 클라우드 최적화 모델 (오로라 등)

## 데이터베이스 타입

- RDBMS (SQL/OLTP) RDS, 오로라 (관계형)
- NoSQL DB DDB(~JSON), ElastiCache(Key/Value), Neptune(Graphs), DocumentDB(for MongoDB), Keyspace(Cassandra)
- Object 저장소 S3 / Glacier (아카이빙)
- Data Warehouse Redshift(OLAP), Athena, EMR
- Search OpenSearch(JSON), 텍스트 검색
- Graphs Neptune
- Ledger AQLD (Amazon Quantum Ledger Database)
- Time series Timestream

## RDS

- 관리형 PostgreSQL / MySQL / MSSQL / MariaDB
- RDS 인스턴스의 크기와 EBS의 크기를 프로비전 (미리 할당)
- 용량은 자동으로 스케일링
- 복수 AZ에 대한 읽기 복제본 생성
- IAM을 통한 보안, Security Group (방화벽), KMS와 SSL을 통한 저장, 전송 중 암호화
- 35일간의 백업 보존을 통한 특정 시점 복구 지원
- DB 스냅샷을 통한 영속적 저장
- 유지보수 관리 시간을 통한 지정된 시간 동안의 백엔드 업데이트
- IAM 인증과 Secret Manager를 통한 권한 관리
- RDS Custom 기능을 통해 백엔드 인스턴스에 SSH로 직접 접근 가능 (MSSQL / Oracle)
- 관계형 데이터베이스에 최적화 (RDBMS / OLTP) SQL쿼리 실행, 트랜잭션 등

## Aurora

- PostgreSQL / MySQL에 호환성을 가진 분리형 클라우드 네이티브 DB 서비스
- 스토리지: 데이터는 6개의 복제본으로 저장되고 3개의 AZ에 나누어짐 (고가용성, 자가복구, 오토 스케일)
- 컴퓨팅: 다중 AZ에 배포된 클러스터형 구조, 읽기 복제본의 오토 스케일
- 클러스터: 읽기/쓰기 엔드포인트로의 접근을 이용한 클러스터링
- RDS와 동일한 보안/모니터링/유지보수 기능
- 포인트 투 타임 리커버리를 지원 (35일간)
- 스냅샷을 통한 영구보존 가능

### 오로라 서버리스

- 예측 불가능한 워크로드, 용량 지정 없이 운영 가능

### 오로라 멀티 마스터

- 여러개의 쓰기 인스턴스를 두고 상호간에 복제를 통해 쓰기 분산 및 장애대처

### 오로라 글로벌

- 최대 16개의 읽기 복제본을 각 리전에 배포하여 상호간의 복제 (<1초단위 복제)

### 오로라 머신 러닝

- SageMaker와 Comprehend를 오로라 쿼리에 직접 적용할 수 있도록 서비스 제공

### 오로라 데이터 클론

- 특정 시점의 오로라 데이터를 빠르게 복제하여 테스트나 스테이징 환경에서 활용할 수 있도록 지원

### 결론

- RDS와 동일하지만, 좀 더 유지보수 시간/유연성/퍼포먼스/다기능을 통해 이점을 가져가는 클라우드 네이티브 RDBMS
