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
