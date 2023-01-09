# Identity and Access Management Advanced

## Sample OU

![images/identity_and_access_management_advanced/1.png](images/identity_and_access_management_advanced/1.png)

## AWS Organizations

- 글로벌 서비스
- 복수 AWS 계정을 관리할 수 있도록 도움을 주는 서비스
- 메인 계정이 관리 계정(management account)이 됨
- 다른 계정은 멤버 계정(member accounts)으로 소속됨
- 멤버 계정은 하나의 organization에 속할 수 있음 (복수 가입 불가)
- 모든 계정에서 발생하는 비용을 하나로 통합 가능
- 사용량에 따른 과금 방식이 하나로 통합되어 사용량 이점을 얻을 수 있음 (볼륨 할인)
- 예약 인스턴스와 절약 플랜을 전 계정에 통합으로 적용 가능
- AWS 계정 생성에 대한 자동화 API를 이용 가능하게 됨

![images/identity_and_access_management_advanced/2.png](images/identity_and_access_management_advanced/2.png)

### 이점

- 복수 계정 vs 하나의 계정안에 다중 VPC 운용
- 태깅을 이용한 청구 관리
- 모든 계정의 CloudTrail 적용 및 중앙 S3 계정으로의 로그 송신
- CloudWatch Logs의 중앙화
- 관리 목적의 크로스 계정 역할 배포

### 보안 (Service Control Policies: SCP)

- OU나 계정에 대한 통합 IAM 정책 적용
- 관리 계정에는 적용되지 않음 (최고 관리자 권한)
- IAM처럼 명시적 허용이 필요 (기본 거부)

## SCP 상속도

![images/identity_and_access_management_advanced/3.png](images/identity_and_access_management_advanced/3.png)

### 관리 계정

- 어떤 것도 할 수 있음 (SCP 적용 안됨)

### Account A

- 어떤 것도 할 수 있음 (FullAWSAccess) Root OU 정책
- Redshift에 대한 접근 거부 (DenyRedshift) Prod OU 정책

### Account B

- 어떤 것도 할 수 있음 (FullAWSAccess) Root OU 정책
- Redshift에 대한 접근 거부 (DenyRedshift) Prod OU 정책
- Lambda에 대한 접근 거부 (DenyAWSLambda) HR OU 정책

### Account C

- 어떤 것도 할 수 있음 (FullAWSAccess) Root OU 정책
- Redshift에 대한 접근 거부 (DenyRedshift) Prod OU 정책
- Lambda에 대한 접근 거부 (DenyAWSLambda) Finance OU 정책

## SCP 샘플

![images/identity_and_access_management_advanced/4.png](images/identity_and_access_management_advanced/4.png)

- 블랙리스트
- 화이트리스트
