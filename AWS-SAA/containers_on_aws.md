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

## Data Volumes (EFS)

- ECS는 EFS와 궁합이 좋은데, 각각의 EC2 인스턴스 내의 태스크들이 모두 볼륨으로 EFS를 사용하게 되면 전부 공유하는 스토리지로 이용 가능하기 때문임
- 또한 EFS도 서버리스이기 때문에 Fargate와 함께 사용하면 인프라에 대한 걱정 없이 서비스를 운영 가능
- 사용 사례
  - 복수 AZ에 대한 가용성 높은 공유 스토리지를 이용하는 서비스
- S3은 서비스에 볼륨으로 사용할 수 없음 (오브젝트형 스토리지)

## ECS Service Auto Scaling

- 자동으로 ECS 숫자를 늘리거나 줄이는 기능
- AWS Application Auto Scaling을 이용
  - ECS Service Average CPU Utilization
  - ECS Service Average Memory Utilization (Scale on RAM)
  - ALB Request Count Per Target (ALB의 지표를 이용)
- 타겟 트래킹
  - CloudWatch에 등록된 지표에 의해
- 스텝 스케일링
  - CloudWatch에 등록된 지표의 알람에 의해
- Scheduled Scaling
  - 정해진 시간에
- ECS에서 스케일링을 한다고 EC2 인스턴스가 증가하는것은 아님
- 설정 CPU와 RAM에 따라 EC2 내에 여러개의 태스크를 가동할 수 있기 때문임

## EC2 Launch Type - Auto Scaling EC2 Instances

- ECS 백엔드 인스턴스를 ECS 서비스가 오토 스케일함
- Auto Scaling Group Scaling
  - CPU 사용량에 따라서 ASG가 자동으로 인스턴스를 조절
- ECS Cluster Capacity Provider
  - ECS 태스크를 기동할때 자동으로 인프라를 관리
  - Capacity Provider가 ASG와 연계되어 있음

![images/containers_on_aws/8.png](images/containers_on_aws/8.png)

## EventBridge에 의해 트리거되는 ECS 태스크

![images/containers_on_aws/9.png](images/containers_on_aws/9.png)

- Amazon EventBridge가 ECS 태스크를 실행할 수 있기 때문에, 서버리스 환경을 이용하여 S3 버킷에 파일을 업로드 했을때, 해당 파일에 대한 처리를 수행할 수 있음
- 또한 태스크 IAM 역할을 이용하면 그때 수행되는 태스크에 필요한 권한을 부여할 수 있음

![images/containers_on_aws/10.png](images/containers_on_aws/10.png)

- EventBridge는 스케줄링에 의해 시간단위로 태스크 실행이 가능하기 때문에, 이를 이용해서 정기적으로 배치 작업을 수행 가능

![images/containers_on_aws/11.png](images/containers_on_aws/11.png)

- SQS 큐에 들어와있는 메시지 수를 CloudWatch를 통해 지표로 설정하고, 해당 지표에 따라서 ECS 태스크의 Auto Scaling이 동작하게 설정 가능
- 따라서 큐에 작업이 밀려있을 경우 태스크를 증가시켜서 처리량을 올릴 수 있게 됨

## Amazon ECR

- ECR (Elastic Container Registry)
- AWS에 도커 이미지를 저장
- 프라이빗과 퍼블릭이 존재 (퍼블릭은 AWS에서 제공하는 퍼블릭 갤러리)
- ECS와 완벽하게 연동되며, S3이 백엔드 저장소로 사용되고 있음
- IAM에 의한 접근제어 정책
- 이미지 취약점 검사, 버전 관리, 이미지 태깅, 라이프사이클(만료) 관리 등을 지원

![images/containers_on_aws/12.png](images/containers_on_aws/12.png)
