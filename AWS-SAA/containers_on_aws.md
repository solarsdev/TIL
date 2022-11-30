# Containers on AWS

## Docker

- Docker는 어플리케이션을 배포하기 위한 개발 플랫폼
- 어플리케이션은 컨테이너에 패키징되며 어떤 OS에서도 동작할 수 있음
- 어플리케이션을 어떤 환경에도 종속적이지 않게 실행 가능
  - 머신 종류
  - 호환성 이슈
  - 예측 행동
  - 적은 작업
  - 유지보수와 배포의 편리함
  - 어떤 언어/OS/기술과도 호환 가능
- 사용 사례
  - 마이크로서비스 아키텍쳐
  - 온프레미스에서 클라우드로의 전환

## OS상에서 Docker 실행 도식

![images/containers_on_aws/1.png](images/containers_on_aws/1.png)

## 도커 이미지 저장소

- 도커 이미지는 도커 리포지토리에 저장됨
- Docker Hub
  - 퍼블릭 리포지토리
  - 많은 베이스 이미지들이 이미 배포되고 있음 (Ubuntu, MySQL 등)
- Amazon ECR
  - 프라이빗 리포지토리
  - 퍼블릭 리포지토리 (Amazon에서 제공하는 공식 퍼블릭 리포지토리)

## 도커와 가상머신의 차이

- 도커는 가상머신과 비슷하지만 똑같지는 않음
- 가상머신이 호스트OS에 하이퍼바이저로 각각의 게스트OS를 생성하는 반면, 도커는 호스트OS의 리소스를 공유하며 도커 데몬이 어플리케이션만을 격리시켜 다른 환경처럼 꾸며줌

![images/containers_on_aws/2.png](images/containers_on_aws/2.png)

## 도커 사용까지 흐름

![images/containers_on_aws/3.png](images/containers_on_aws/3.png)

## AWS에서 도커 컨테이너를 관리하는 서비스들

- Amazon Elastic Container Service (Amazon ECS)
  - 아마존의 컨테이너 플랫폼
- Amazon Elastic Kubernetes Service (Amazon EKS)
  - 아마존의 관리형 쿠버네티스 플랫폼 (오픈소스 기반)
- AWS Fargate
  - 아마존의 독자 서버리스 컨테이너 플랫폼
  - ECS 또는 EKS에 레버리징 가능
- Amazon ECR
  - 프라이빗 컨테이너 리포지토리

## ECS (EC2 Launch Type)

- 도커 컨테이너를 AWS에서 구현하는 방식은 ECS Task를 ECS Cluster에서 실행하는 것
- ECS Cluster에는 두가지 방식이 존재하는데 그 중 EC2 인스턴스에 ECS 에이전트를 설치하여 실행하는 방식임
- EC2 인스턴스를 미리 구성하여 해당 EC2 인스턴스에 도커 기반 ECS 에이전트를 설치
- 각각의 EC2 인스턴스는 ECS 클러스터의 구성요소로 등록됨
- ECS 클러스터가 각 EC2에서 운영되고 있는 ECS 에이전트에 명령을 내리고, 태스크를 올림
  - 태스크 = 이미지

![images/containers_on_aws/4.png](images/containers_on_aws/4.png)

## ECS (Fargate Launch Type)

- 백엔드 인프라를 관리할 필요 없이 AWS에서 관리형 서비스로 운영
- 태스크에 대한 정의와 세부 사항만을 제공하면 해당 요구사항에 맞추어 인프라를 구성해줌

![images/containers_on_aws/5.png](images/containers_on_aws/5.png)

## IAM Role for ECS

- EC2 인스턴스 프로필 (EC2 실행 타입)
  - ECS 에이전트에서 사용되는 역할
  - ECS 서비스에 API 요청에 대한 권한을 컨트롤
  - CloudWatch Logs에 로그를 저장하거나, ECR에서 이미지를 풀링하거나, 설정 파일/패스워드 등과 같이 민감한 정보들을 Secret Manager나 Parameter Store등에서 읽어들일때 필요한 권한등을 명시
- ECS 태스크 역할
  - 각각의 태스크마다 필요한 권한을 별도로 명시

![images/containers_on_aws/6.png](images/containers_on_aws/6.png)

## Load Balancer Integrations

- 어플리케이션 로드 밸런서가 지원되며 일반적으로 이용됨
- 네트워크 로드 밸런서는 고성능의 네트워크나 처리량을 요구할때 추천됨
- CLB는 추천되지 않음 (포트 맵핑등을 지원안하고, Fargate에서 이용 불가)

![images/containers_on_aws/7.png](images/containers_on_aws/7.png)
