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

## CloudWatch Logs

- 로그 그룹
  - 로그 집합의 이름, 일반적으로는 어플리케이션의 이름을 선정
- 로그 스트림
  - 어플리케이션/로그 파일/컨테이너의 인스턴스 (디멘션)
- 로그 만료 기간 설정
  - 로그가 기록되고 나서 얼마나 보존할지 기한을 설정 (만료 없음부터 최대 10년까지)
- CloudWatch 로그는 다양한 곳으로 보내거나/저장 가능
  - AWS S3 (추출)
  - Kinesis Data Streams (실시간 연계)
  - Kinesis Data Firehose (엔드포인트 송신)
  - AWS Lambda (변환)
  - ElasticSearch (키밸류 검색엔진)

## CloudWatch Logs Sources

- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: 어플리케이션 로그 수집
- ECS: 컨테이너 로그 수집
- AWS Lambda: 함수 실행 기록 (로그, 에러 검증)
- VPC Flow Logs: VPC 전용 메타데이터 로그
- API Gateway: 요청 로그
- CloudTrail based on filter: 특정 설정에 따른 감사 기록 로그
- Route53 Logs: DNS 쿼리 기록

## CloudWatch Logs Metric Filter & Insights

- 클라우드워치 로그는 필터링 식이 존재하며 이용 가능
  - 예를 들면 특정 아이피에 대한 로그만
  - 에러 코드나 특정 에러 텍스트에 대한 검출 등
- 지표 필터를 이용하여 해당 로그가 발생했을 때 클라우드워치 알람과 연동 가능
- 클라우드워치 로그 인사이트는 특정 쿼리를 지정해두고 대시보드에서 게속 활용하고 시각화 할 수 있도록 하기 위함

## CloudWatch Logs - S3 Export

- 로그 데이터는 추출을 위해 최대 12시간까지 대기
- API요청은 CreateExrportTask
- 실시간 추출은 불가능 (로그 구독을 대신 활용할것)

## CloudWatch Logs Subscriptions

![images/monitoring_audit_and_performance/2.png](images/monitoring_audit_and_performance/2.png)

- 클라우드워치 로그를 실시간으로 필터링하여 다양한 엔드포인트 및 서비스로 송신할 수 있음
- 또한 다양한 계정의 각기 다른 리전으로부터의 로그를 하나의 데이터 스트림을 통해 Firehose로 S3에 모아둘 수도 있음

![images/monitoring_audit_and_performance/3.png](images/monitoring_audit_and_performance/3.png)

## CloudWatch Logs for EC2

- 디폴트로 EC2에서 클라우드워치로의 로그를 보내는 기능은 없음
- CloudWatch 에이전트를 설치하여 로그 파일을 전송 가능
- IAM 권한을 적절하게 설정해야 함
- CloudWatch Logs 에이전트는 온프레미스 환경에도 설치 가능

![images/monitoring_audit_and_performance/4.png](images/monitoring_audit_and_performance/4.png)

## Logs Agent와 Unified Agent

- EC2 인스턴스나 온프레미스 서버에 설치 가능
- Logs Agent
  - 오래된 버전 (권장되지 않음)
  - CloudWatch Logs에 로그를 보내는 기능만 존재
- Unified Agent
  - RAM이나 프로세스 등을 보내는 기능도 추가되어 있음
  - 로그를 수집하여 CloudWatch Logs에 전송 가능
  - SSM Parameter Store를 이용하여 설정을 중앙화 관리 가능

## CloudWatch Unified Agent 지표

- CPU (활성화, 게스트, 유휴, 시스템, 유저 단위)
- Disk 지표 (가용, 사용중, 전체), Disk IO (쓰기, 읽기, 바이트 IOPS 등)
- RAM (가용, 사용중, 전체, 캐시)
- Netstat (TCP 또는 UDP 커넥션, 패킷 수, 인아웃 바이트)
- 프로세스 (전체, 종료, 유휴, 가동중, 슬립)
- Swap 용량 (가용, 사용중, 전체 사용량 %)
- EC2의 기본 지표와는 다른 추가적인 지표 수집 가능

## CloudWatch Alarms

- 알람은 어떤 지표든 이용하여 트리거될 수 있음
- 다양한 옵션을 제공 (샘플링, %, 최대 최소, 기타)
- 알람 상태
  - OK
  - INSUFFICIENT_DATA (지표 데이터 부족)
  - ALARM
- 기간
  - 상태가 평가될때까지의 시간
  - 커스텀 지표를 통한 고해상도 설정: 10초, 30초 또는 60초의 배수

## CloudWatch Alarm Targets

- EC2 인스턴스에 대한 정지, 삭제, 재기동, 복구 등
- ASG에 대한 트리거
- SNS로 메시지 발행 (이것과 연계한 다양한 액션들)

### Composite Alarm

- 알람은 하나의 지표를 통해 설정됨
- 컴포지트 알람은 여러개의 지표 상태를 등록할 수 있음
- AND 또는 OR 연결
- 특정 상태에 대해서만 알람을 설정하고 싶을 때 유용하게 활용될 수 있음

![images/monitoring_audit_and_performance/5.png](images/monitoring_audit_and_performance/5.png)

## EC2 인스턴스 복구

- 상태 검사
  - 인스턴스 상태 = EC2의 VM
  - 시스템 상태 = 백엔드 하드웨어
    ![images/monitoring_audit_and_performance/6.png](images/monitoring_audit_and_performance/6.png)
- 복구
  - 같은 프라이빗, 퍼블릭 Elastic IP, metadata, 배치 그룹 등

## 기타 CloudWatch Alarm 정보

- 알람은 Logs 지표 필터에 의해 활성화 될 수 있음

![images/monitoring_audit_and_performance/7.png](images/monitoring_audit_and_performance/7.png)

- 알람을 수동으로 설정할 수 있음

```bash
aws cloudwatch set-alarm-state --alarm-name "myAlarm" --state-value ALARM --state-reason "testing purpose"
```

## Amazon EventBridge

- 원래 Amazon CloudWatch Events에서 기능이 강화되면서 명칭이 변경됨
- 스케줄링: 크론 작업 (일정화 스크립트)

![images/monitoring_audit_and_performance/8.png](images/monitoring_audit_and_performance/8.png)

- 이벤트 패턴: 이벤트 발생 베이스로 특정 작업을 수행

![images/monitoring_audit_and_performance/9.png](images/monitoring_audit_and_performance/9.png)

- 람다 함수를 트리거하거나 SQS/SNS 메시지를 발행

![images/monitoring_audit_and_performance/10.png](images/monitoring_audit_and_performance/10.png)

- 각종 이벤트 소스로부터의 이벤트를 필터링(옵션)하여 이벤트 브리지는 다양한 AWS 타겟 서비스에 트리거 및 패러미터 전달자로서의 역할을 수행
- 전달되는 패터리터는 JSON 형식