# Monitoring, Audit and Performance

## CloudWatch Metrics

- AWS의 모든 서비스에 대한 지표를 제공
- 지표는 모니터링하는 변수 (CPU사용률, 네트워크 패킷 수 등)
- 지표는 네임스페이스에 속함
- 디멘션(차원)은 지표의 속성값 (인스턴스ID, 환경, 기타)
- 하나의 지표당 10개의 디멘션까지 분류 가능
- 지표는 타임스탬프가 있어 시계열로 나열 가능
- 클라우드워치 대시보드를 이용하여 지표를 한데 표현할 수 있음
- 커스터 지표를 이용하면 기본적으로 지원하는 지표 외에 것을 수집할 수 있음 (램 사용량은 기본 지표가 아니며, 이는 커스텀 지표를 사용하는 가장 흔한 예)

## Metric Streams

- 클라우드워치의 지표는 클라우드워치 뿐만 아니라 다른 서드파티 모니터링 엔진에도 거의 실시간으로 보낼 수 있음
- 이는 Kinesis Data Firehose를 통해서 1MB 혹은 60초당 메시지를 보낼 수 있기 때문에 거의 실시간임
- 옵션으로 필터링을 통해 원하는 조건의 데이터만 스트림 가능

![images/monitoring_audit_and_performance/1.png](images/monitoring_audit_and_performance/1.png)
