# Kinesis 101

- 키네시스는 실시간으로 데이터를 수집하고, 처리하고, 분석하는 서비스
- 키네시스 데이터 스트림: 데이터 스트림을 수집, 처리, 분석
- 키네시스 데이터 파이어호스: AWS 데이터스토어에 데이터 스트림을 적재
- 키네시스 데이터 분석: SQL이나 아파치 플링크를 이용해서 데이터 스트림을 분석
- 키네시스 비디오 스트림: 비디오 스트림을 수집, 처리, 저장

## Kinesis Data Streams

- 키네시스 데이터 스트림의 구조
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f80e0f2-7413-403e-9bd0-b420fe1bd0e8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f80e0f2-7413-403e-9bd0-b420fe1bd0e8/Untitled.png)
- 청구는 샤드 단위로 이루어진다. 샤드는 원하는 만큼 만들 수 있다.
- 보존기간은 기본 1일이며, 365일까지 늘릴 수 있다.
- SQS와는 다르게, 키네시스에서는 데이터를 변경하고 재전송할수 있다.
- 데이터가 키네시스에 입력되면, 데이터는 지워지지 않는다. (데이터불변)
- 같은 파티션키로 입력된 데이터는 같은 샤드를 통해서 이동하므로, 같은 파티션키를 사용하면 순서가 보장된다.
- 공급자: AWS SDK, 키네시스 공급자 라이브러리 (KPL), 키네시스 에이전트
- 소비자
  - 키네시스 소비자 라이브러리 (KCL), AWS SDK
  - 관리형 서비스: AWS Lambda, Kinesis Data Firehose, Kinesis Data Analytics

## Kinesis Data Stream 보안

- IAM 정책을 통한 접근제어, 인증
- HTTPS를 통한 전송 시 암호화
- 저장된 데이터의 KMS를 통한 암호화
- 클라이언트측 암호화/복호화 지원
- VPC 엔드포인트를 이용한 VPC에서 인터넷 경유없이 다이렉트 접속

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87f29050-16eb-4ce6-a2f8-8de73931d2b9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87f29050-16eb-4ce6-a2f8-8de73931d2b9/Untitled.png)

- CloudTrail을 통한 API 요청 감사

## Kinesis 공급자

- 데이터 스트림으로 데이터 레코드를 올린다.
- 데이터 레코드는 다음을 보증
  - 시퀀스 번호 (같은 파티션 키를 사용하는 데이터의 순서 번호)
  - 파티션 키 (스트림으로 데이터를 올릴 때 반드시 정의되어야 함)
  - 데이터 블롭 (1MB까지)
- 공급자
  - AWS SDK: 심플
  - Kinesis Producer Library (KPL): C++. Java, batch, compression, retires
  - Kinesis Agent: 로그 파일 모니터링
- 쓰기 대역폭: 초당 1MB 혹은 샤드당 초당 1000레코드
- PutRecord API 요청
- PutRecord API는 한번에 여러개의 레코드를 대상으로 처리 가능하므로 복수개의 레코드를 한번에 처리해서 비용을 절감할 수 있음
- 파티션키를 정하는것에는 신중해야 함. 파티션 키당 하나의 샤드가 되기 때문에, 잘 배분되지 않은 키를 선정할 경우 특정 샤드에만 데이터가 몰릴 수 있음.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/315297fa-9c28-45a0-93ea-2ba90a073c58/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/315297fa-9c28-45a0-93ea-2ba90a073c58/Untitled.png)

## ProvisionedThroughputExceeded

- 특정 파티션키에 몰려서 허용량이 초과될 경우 대책
  - 잘 배분된 파티션 키를 사용
  - 지수 백오프 기능을 이용해서 다시 시도
  - 샤드를 늘려서 스케일을 늘림
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b422d2a1-f388-4273-9574-a4ab18202578/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b422d2a1-f388-4273-9574-a4ab18202578/Untitled.png)

## Kinesis 소비자

- 데이터 스트림으로부터 레코드를 가져와서 처리함
- 람다
- 데이터 애널리틱스
- 데이터 파이어호스
- 커스텀 소비자 (SDK) 클래식 또는 강화된 팬아웃
- 키네시스 클라이언트 라이브러리 KCL : 데이터 스트림으로부터 읽어들이는 간단한 라이브러리

## 키네시스 소비자 : 커스텀 소비자 개념

- 공유 팬아웃 소비자
  - 특정 샤드의 접근하는 모든 소비자가 2MB/s 트래픽을 공유함
- 강화된 팬아웃 소비자
  - 샤드당 접근하는 소비자당 2MB/s를 이용
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/915a7a7e-a011-4bef-a917-687159a9cb0c/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/915a7a7e-a011-4bef-a917-687159a9cb0c/Untitled.png)
- 클래식 팬아웃은
  - 적은 숫자의 소비자
  - 모든 소비자당 2MB/s로 충분한 경우
  - 최대 초당 5 GetRecords API 요청
  - ~200ms 응답속도
  - 비용 최소화
  - GetRecords API를 통한 폴링
- 강화된 팬아웃 소비자 개념
  - 같은 스트림의 많은 소비자가 동시 이용
  - 소비자당 2MB/s의 독립된 트래픽 이용
  - ~70ms 응답속도
  - 높은 비용
  - HTTP/2를 이용한 데이터 푸시
  - 소프트 제한으로 데이터 스트림당 5개의 소비자만 접근 가능하지만 AWS에 요청해서 늘릴수 있음

## 키네시스 소비자 모델 : 람다

- 클래식과 강화된 모델을 둘 다 지원
- 배치를 통해 레코드를 읽어옴
- 배치 사이즈와 배치 윈도우를 설정해서 조절 가능
- 에러가 발생하면 람다는 성공할때까지 혹은 데이터가 만료될때까지 계속 재시도 함
- 하나의 샤드에 동시에 10개의 배치를 처리할 수 있음
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fb26029-6904-471e-a3a4-6b7b402e8191/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6fb26029-6904-471e-a3a4-6b7b402e8191/Untitled.png)

## 키네시스 클라이언트 라이브러리

- 키네시스 데이터 스트림으로부터 레코드를 읽어올 수 있게 도와주는 자바 라이브러리로, 분산 환경이고 읽기 작업을 나눌 수 있다.
- 각각의 샤드에서 읽어들이는 KCL 인스턴스는 샤드당 하나이다.
  - 4 샤드 = 4 KCL 인스턴스
  - 6 샤드 = 6 KCL 인스턴스
- 작업 진척도는 DynamoDB에 보존된다 (IAM 접근정책 필요)
- DynamoDB를 이용해서 다른 작업자와 작업 내용을 공유한다
- KCL은 EC2, 엘라스틱 빈스토크, 온프레미스 서버 등에서 돌릴 수 있다
- 레코드는 샤드 레벨에서 순서를 가지고 읽힌다
- 버전
  - KCL 1.x (공유 소비자 모델을 지원)
  - KCL 2.x(공유, 강화된 소비자 모델을 지원)
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/db770bb8-d27e-4694-9639-6bd1a2df9a96/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/db770bb8-d27e-4694-9639-6bd1a2df9a96/Untitled.png)
  ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0536e139-33fa-4bb0-8cd2-a42ff7d7c0b7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0536e139-33fa-4bb0-8cd2-a42ff7d7c0b7/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/434628d8-ecbe-4d93-99ad-162ce7733634/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/434628d8-ecbe-4d93-99ad-162ce7733634/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6721ccd-c6a6-496b-85e3-4f2ba34a5d1d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6721ccd-c6a6-496b-85e3-4f2ba34a5d1d/Untitled.png)

## 키네시스 운영 - 샤드 나누기

- 스트림 용량을 늘리기 위해 ( 1mb/s 데이터 쓰기 샤드당)
- 핫 샤드를 나누기 위함

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d7d6d0fc-2584-4870-aed0-c1965b616a9a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d7d6d0fc-2584-4870-aed0-c1965b616a9a/Untitled.png)

- 오래된 샤드는 데이터가 만료되면 삭제된다. (데이터 보존기간을 얼마로 설정해두었느냐에 따라 다름)
- 오토 스케일링은 없음
- 한번에 두개가 넘게 쪼갤순 없음

## 키네시스 운영 - 샤드 합치기

- 비용을 절감하기 위해 스트림 용량을 줄이기
- 두개의 샤드를 합침 (콜드 샤드)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9dacaaa7-07e5-4cdd-9a9a-2053ce0f02f1/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9dacaaa7-07e5-4cdd-9a9a-2053ce0f02f1/Untitled.png)

- 오래된 샤드들은 데이터가 만료되면 삭제됨
- 한번에 두개가 넘는것을 하나로 합칠 수 없음

## 키네시스 파이어호스

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dea9106-49fe-4d2f-b47e-25ea75d835a0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8dea9106-49fe-4d2f-b47e-25ea75d835a0/Untitled.png)

- 완전 관리형 서비스, 관리가 필요없고 자동으로 스케일업되며 서버리스이다
  - AWS 레디시프트 S3 엘라스틱서치
  - 그 외 서드파티 들
  - 커스텀 : HTTP 엔드포인트
- 데이터를 보낸 만큼만 과금
- 거의 리얼타임
  - 최소 60초는 필요함 (버퍼 관련) ~900초
  - 1~128MB의 데이터 버퍼
  - 둘중 하나를 먼저 만족하면 버퍼가 푸쉬됨
- 다양한 데이터 포맷을 지원, 변환, 변환, 압축 또한 지원
- AWS 람다를 이용한 원하는 거의 모든 작업이 가능
- 전송에 실패한 데이터들을 S3에 따로 저장도 가능

## 키네시스 데이터 스트림 vs 파이어호스

- 키네시스 데이터 스트림은
  - 정해진 스케일의 데이터를 스트리밍할때 유용
  - 코드를 작성해야됨 (공급자, 소비자 모델)
  - 리얼타임 (~200ms)
  - 스케일을 정할 수 있음 (샤드 분할, 통합)
  - 데이터는 1일~365일 보존가능
  - 재전송 지원
- 키네시스 파이어호스
  - 데이터를 S3, 레드시프트, 3rd 파티, HTTP등으로 바로 전송
  - 완전 관리형
  - 거의 리얼타임 (버퍼를 위해 최소 60초는 필요함)
  - 자동으로 스케일링됨
  - 데이터 보관은 하지 않음
  - 재전송 지원하지 않음

## Kinesis Data Analytics (SQL 어플리케이션)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d829d1e4-ae18-46c4-a41a-f92e692a2329/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d829d1e4-ae18-46c4-a41a-f92e692a2329/Untitled.png)

- SQL을 이용한 키네시스 스트림을 리얼타임 분석하기 위한 도구
- 완전 관리형 서비스
- 자동으로 스케일링됨
- 리얼타임 분석
- 실제 소비된 비율에 따라서 과금
- 리얼타임 쿼리 스트림을 생성 가능
- 유스케이스
  - 시간기반 분석
  - 리얼타임 대시보드
  - 리얼타임 지표

## SQS vs SNS vs Kinesis

- SQS
  - 소비자는 데이터를 풀링한다
  - 데이터는 소비된 뒤에 삭제된다
  - 원하는 만큼의 소비자를 사용할 수 있다
  - 대역폭을 미리 지정할 필요 없다
  - FIFO 큐에서는 순서가 보장된다
  - 개별 메시지 딜레이 기능
- SNS
  - 많은 구독자들에게 메시지를 보낼 수 있다
  - 12,500,000 구독자 가능
  - 데이터는 저장되지 않고 한번 보낼때 실패하면 손실된다
  - 발행자 구독자 모델
  - 100,000개의 토픽을 만들 수 있음
  - 대역폭을 미리 지정할 필요 없음
  - SQS와 결합하여 팬아웃 아키텍쳐 모델을 사용 가능
  - FIFO의 수용량은 SQS FIFO에 의해 제어됨
- 키네시스
  - 기본 : 풀링 데이터
    - 샤드당 2MB
  - 강화 : 풀링 데이터
    - 샤드에 붙은 클라이언트당 2MB
  - 데이터 재전송 가능
  - 실시간 빅데이터나 분석 그리고 ETL에 어울림
  - 샤드 레벨에서 순서가 보장
  - X일간의 데이터 만료일 지정기능
  - 처리량은 미리 지정해야함

## 키네시스에서 데이터 순서 에 대해서

- 키네시스에서는 파티션키를 해시해서 샤드를 정한다.
- 그 이후에 동일한 파티션키로 오는 데이터는 같은 샤드를 이용하게 된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a585b6c-9569-4d36-8a44-7a45b825cc6b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9a585b6c-9569-4d36-8a44-7a45b825cc6b/Untitled.png)

## SQS에서의 데이터 순서에 대해서

- SQS 스탠다드에서는 순서가 없음
- SQS FIFO에서는 그룹아이디를 사용하지 않으면 메시지는 보내진 순서대로 소비된다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c05f9a0-0680-4132-861d-1265b361f669/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7c05f9a0-0680-4132-861d-1265b361f669/Untitled.png)

- FIFO큐에서 그룹 아이디를 지정하지 않으면 하나의 큐에만 모든 메시지가 징집되므로 하나의 소비자만 지정할 수 있다
- 소비자의 숫자를 스케일업하고 싶으면 그룹으로 묶어서(메시지당 그룹아이디를 별도로 지정해서) 처리하면 된다.
- 이렇게 되면 그룹아이디를 지정해서 별도의 처리가 가능하고, 이는 키네시스의 파티션키와 비슷함

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8ee2bea-94fa-4cb8-b265-1a9a3551209f/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c8ee2bea-94fa-4cb8-b265-1a9a3551209f/Untitled.png)

## Kinesis vs SQS 순서처리

- 100대의 트럭, 5개의 샤드를 가진 키네시스, 1개의 SQS FIFO큐에서 어떻게 처리할까
- 키네시스 데이터 스트림
  - 1개의 샤드당 대략 20개의 트럭정도가 배분될 것이다
  - 트럭은 각각의 샤드에서는 순서에 맞게 데이터를 수신할수 있다
  - 최대 병렬처리 가능한 소비자 수는 샤드의 갯수와 비례하므로, 최대 5개의소비자를 가질 수 있다
  - 그리고 데이터는 최대 5MB/s로 처리될 것이다
- SQS FIFO
  - SQS FIFO큐는 하나만 가질 수 있다
  - 데이터를 메시지 아이디를 지정해서 그룹으로 나눌수 있기 때문에 100개를 개별메시지 아이디로 지정할 수 있다
  - 그러면 그룹당 1개의 소비자를 지정가능하기 때문에 100개를 동시에 병렬로 처리할수 있다
  - 다만 초당 300개의 메시지만 처리할수 있으며 배치처리를 통해 10개의 데이터를 동시에 받아온다면, 이론상으로 3000개의 메시지를 초당 처리할 수 있다
