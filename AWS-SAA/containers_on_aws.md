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
