# Data & Analytics

## Athena

- S3 데이터에 직접 쿼리를 수행할 수 있는 서버리스 서비스
- Presto 기반으로 파일 내용에 SQL을 수행 가능
- CSV, JSON, ORC, Avro, Parquet 등의 파일을 지원
- 스캔된 데이터 1TB당 $5.0
- 아마존 퀵사이트에 리포트/대시보드 표시에 주로 이용됨
- 사용사례
  - 비지니스 인사이트/분석
  - 보고서/분석 및 쿼리 (각종 데이터 소스에 대한)

![images/data_and_analytics/1.png](images/data_and_analytics/1.png)

## Athena 퍼포먼스 개선

- columnar 데이터 타입을 추천 (비용 절감)
  [[개념][데이터베이스] 칼럼지향 데이터베이스(columnar database)](https://118k.tistory.com/400)
  - 아파치 parquet 또는 ORC가 권장됨
  - 큰 성능 증가
  - Glue를 이용하면 데이터를 Parquet 또는 ORC로 변환 가능
- 데이터 압축을 통한 반환 데이터 축소 (bzip2, gzip, lz4, snappy, zlip, zstd)
- 데이터셋의 파티셔닝을 통한 쿼리의 간략화
- 큰 파일일 수록 효율적임

## Athena 통합 쿼리

- 아테나는 커스텀 데이터 소스를 이용하여 상호간의 조인을 허용
- 예를 들어, 람다를 이용하여 각종 데이터 소스 (RDS, DDB, Redshift …)등으로부터 데이터를 각각 가져옴
- 가져온 데이터들끼리의 조인 등을 통해 결과 도출 가능
- 도출된 데이터는 다시 S3에 저장됨 (아테나 쿼리 결과)

![images/data_and_analytics/2.png](images/data_and_analytics/2.png)

## Redshift Overview

- 레드시프트는 PostgreSQL을 기반으로 했지만 OLTP를 제거한 기술
- OLAP 기능만을 목적으로 하고 있는 서비스
- OLTP
  - 웹 서비스에서 주로 발생하는 트랜잭션 처리
  - 읽기/쓰기/변경/삭제 등이 주로 트랜잭션
  - 실시간 데이터를 처리해서 질의자에게 돌려주는 것을 목적으로 함
- OLAP
  - 쌓여있는 데이터에서 어떠한 가공을 통해 원하는 데이터를 추출하는 것이 목적
  - 분석을 위한 데이터 처리 및 가공이 주된 역할
- 다른 데이터 웨어하우스 서비스보다 성능면에서 큰 이점이 있고, PB단위의 데이터까지 다룰 수 있음
- Columnar 스토리지 (행기반)로 병렬 쿼리 엔진을 가지고 있음
- 레드시프트 인스턴스를 프로비전한 만큼 과금됨
- SQL 인터페이스가 존재하며 쿼리를 수행 가능
- 아마존 퀵사이트 혹은 타블로와 같은 툴과 연계 가능
- 아테나와 비교하여 더 빠른 쿼리 및 조인이 가능한데, 이는 인덱스를 보유하고 있기 때문임

## Redshift Cluster

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/08c58513-a491-4a04-9086-647e5456ae12/Untitled.png)

- 레드시프트는 리더노드와 컴퓨트노드로 나누어짐
- 리더노드는 질의자의 쿼리를 받아주는 노드로 컴퓨트 노드는 리더노드의 지시하에 쿼리를 수행하는 인스턴스
- 각각의 노드는 인스턴스이며, 인스턴스 옵션을 통해 비용 절감 가능

## Redshift 스냅샷 & DR

- 레드시프트는 멀티 리전을 지원하지 않음
- 스냅샷을 통해 S3에 데이터를 저장해야 하는데, 자동화된 백업과 온디맨드 백업을 지원
- 자동화된 백업은 8시간당, 5기가당으로 둘중 만족시키는 조건하에 백업이 진행되고 보존기간은 35일까지 지정 가능
- 온디맨드 백업은 PITR을 지원하고 삭제 전까지 유지 가능
- 레드시프트 자동 백업을 통해 스냅샷을 다른 리전에 자동으로 복제 하는 기능도 설정 가능

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac482025-7d3d-4217-a697-cadef9cb7653/Untitled.png)

## Redshift 데이터 로딩

- 레드시프트에 데이터를 쌓는 방식으로는 3가지 패턴이 있음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8427b3e2-ff8b-4b87-b0d3-0764f653d0d3/Untitled.png)

### Amazon Kinesis Data Firehose

- 파이어호스를 통해 레드시프트 클러스에 직접 연결

### S3 카피 커맨드를 이용하여 레드시프트에 연결

- VPC 인터페이스를 통해 S3과 직접 내부망을 이용하게 하지 않으면 카피 명령어는 인터넷을 통해 수행됨 (느림, 비쌈)

### EC2 인스턴스 JDBC 드라이버

- 인스턴스에 레드시프트 JDBC 드라이버를 이용하여 직접 연결하여 데이터 로딩

## Redshift 스펙트럼

- S3에 있는 데이터를 분석할때 사용
- 레드시프트 스펙트럼이 S3에 병렬적으로 쿼리를 처리하여 컴퓨트노드↔리더노드를 통해 질의자에게 결과를 전달해줌

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67b918ca-936c-453d-ab7a-6eb4ebb1edbc/Untitled.png)
