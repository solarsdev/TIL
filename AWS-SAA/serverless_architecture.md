## MyTodoList

### 요구사항

- REST API를 통한 HTTPS 엔드포인트 노출
- 서버리스 아키텍쳐
- 유저는 S3을 통해 직접 소통 가능할것
- 관리형 서버리스 서비스를 통한 인증
- 유저는 투두를 작성하고, 조회함 (대부분은 조회)
- 데이터베이스는 가끔 많은 읽기를 위해 자동으로 스케일링되어야 함

### Mobile 앱 : REST API 계층

![images/serverless_architecture/1.png](images/serverless_architecture/1.png)

### Mobile 앱 : S3에 권한 부여

![images/serverless_architecture/2.png](images/serverless_architecture/2.png)

### Mobile 앱 : 고성능의 읽기 처리

![images/serverless_architecture/3.png](images/serverless_architecture/3.png)

### Mobile 앱 : 고성능의 읽기 처리 (캐싱 레이어 추가)

![images/serverless_architecture/4.png](images/serverless_architecture/4.png)

### 정리

- 서버리스 REST API
  - HTTPS
  - API Gateway
  - Lambda
  - DynamoDB
- 코그니토를 이용한 임시 크레덴셜 생성으로 STS를 이용, S3에 엑세스 (권한 설정)
- 앱 유저는 AWS에 직접 접속하여 S3 버킷 내용 확인 가능
- DDB와 DAX를 이용한 캐싱 테크 적용
- REST 요청 또한 API Gateway 캐싱을 적용
- 인증과 인가를 코그니토와 STS를 이용하여 보안성 확보

## Serverless hosted website

### 요구사항

- 글로벌 스케일 웹사이트
- 블로그는 가끔 작성되고 대부분 읽기 (자주 엑세스가 발생)
- 웹사이트의 일부 파일은 정적 파일이고, 나머지는 REST API로 동적 접근
- 캐싱이 가능한 곳에서는 적용
- 새로운 유저가 구독을 할 경우 이메일로 알림을 줌
- 업로드된 사진은 썸네일을 작성

### 유저 구독 이메일 (웰컴)

![images/serverless_architecture/5.png](images/serverless_architecture/5.png)

### 썸네일 작성 흐름

![images/serverless_architecture/6.png](images/serverless_architecture/6.png)

### 정리

- S3 정적 컨텐츠를 CloudFront를 이용 글로벌 엣지 로케이션으로 배포 (CDN)
- REST API는 API Gateway를 이용, 퍼블릭이기 때문에 코그니토는 필요 없음
- Global DDB 기능을 이용하여 데이터베이스를 양방향 모든 리전으로 복제
  - RDS를 이용한다면 오로라 글로벌 데이터베이스 이용 가능
- DDB Streams를 이용, 람다 함수로 유저 환영 이메일을 발송 가능
  - 람다 함수에 IAM 역할을 이용하여 SES를 사용
- S3은 SQS / SNS를 호출하여 추가적인 기능 수행 가능 (썸네일 작성)

## Microservices Architecture

### 요구사항

- 마이크로 서비스로의 전환
- 각각의 서비스들이 서로 REST API를 이용해서 소통
- 각각의 마이크로 서비스들은 각자의 사양에 맞게 조금씩 다른 요구사항을 가짐
- 마이크로 서비스들은 다른 개발 라이프사이클을 가질 수 있도록 설계

### 구성도

![images/serverless_architecture/7.png](images/serverless_architecture/7.png)

### 정리

- 서비스에는 다양한 디자인의 아키텍쳐가 등장할 수 있음
- 동기 패턴
  - API Gateway
  - Load Balancer
- 비동기 패턴
  - SQS
  - Kinesis
  - Lambda Triggers
- 마이크로 서비스에서의 도전과제
  - 새로운 마이크로 서비스를 만드는데 필연적인 서비스 생성의 반복
  - 서버 응집도 / 가동지속성 최적화에 대한 이슈
  - 각각의 서비스를 각각의 버전으로 관리하는데 필요한 복잡함
  - 분리된 서비스들 간의 서비스 연계에 필요한 중복 코드 증가
- 서버리스 아키텍쳐로 해결 가능한 도전과제
  - API Gateway, 람다 등을 이용한 자동화된 스케일링 기능 및 사용한 만큼만 과금하는 구조
  - API의 복제가 간단하고, 환경을 재생성하는게 용이
  - SDK를 이용한 Swagger 연동 API 구조 복제
