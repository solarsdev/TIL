# ECS 클러스터에 대해서

- ECS 클러스터는 EC2 인스턴스들의 논리적인 집합
- EC2 인스턴스에 ECS 에이전트를 설치해서 기동시킴 (도커 컨테이너)
- ECS 에이전트는 ECS 클러스터에 인스턴스를 등록함
- EC2 인스턴스는 특별한 AMI로 기동되고 이 AMI는 ECS를 위해 특별히 제작된 것임
  ![images/ecs/1.png](images/ecs/1.png)

## ECS 작업 정의

- 작업 정의는 JSON 폼으로 된 명세서이며, ECS에게 어떤 도커 컨테이너를 실행할것인지 지시하는 것임
- 작업 정의에는 다음과 같은 정보가 포함됨
  - 이미지 이름
  - 컨테이너와 호스트의 포트 맵핑
  - 메모리와 CPU 사용량
  - 환경 변수
  - 네트워킹 정보
    ![images/ecs/2.png](images/ecs/2.png)

## ECS에서 클러스터 만들고 작업 정의를 생성하기

- ECS는 실습에서는 EC2 + 네트워크 기반 타입으로 생성
- ECS 클러스터를 생성하면 AWS의 백그라운드에서는 다음을 자동으로 생성한다
  - 런치 컨픽
    - 런치 컨픽에는 EC2의 부트스트랩 데이터(유저데이터)로서 /etc/ecs/ecs.config에 다음과 같은 정보를 남긴다
    ```bash
    ECS_CLUSTER=cluster-demo
    ECS_BACKEND_HOST=
    ```
  - 오토 스케일링 그룹
  - 설정한 숫자만큼의 EC2
- 작업 정의를 작성
  - 작업 정의는 다음과 같은 중요한 정보를 지정한다
    - CPU와 메모리
    - 도커 허브에서 끌어올 이미지 및 태그 (ECR로 가능)
    - 작업 정의 실행 역할 (작업정의를 실행할때 해당 IAM 역할을 부여받아 수행함)
    - 각종 네트워킹 설정 등
    - 특히 포트 맵핑 (호스트포트 : 컨테이너포트)

## ECS클러스터의 서비스를 중복화 시키는 방법

- 작업 정의로 생성한 서비스의 숫자를 늘리면 즉시 클러스터에서는 새로운 서비스를 기동하려고 시도
- 단, 호스트 포트를 정의해둔 경우 동일 인스턴스에서 동일 포트를 중복사용 불가능하기 때문에, 추가인스턴스를 제공해야 함
- 오토스케일 그룹에서 목표치와 최대치를 증가시켜 인스턴스를 증가시키면 새로운 서비스가 다른 인스턴스에서 기동됨
- 그렇다면 하나의 인스턴스에서 하나의 서비스밖에 기동하지 못하는건가? → 아님, 로드밸런스 이용
- 어플리케이션 로드밸런서는 다이나믹 포트포워딩을 이용해서, 호스트 포트를 지정하지 않을 경우 랜덤으로 포트를 할당하고 해당 포트를 맵핑
  ![images/ecs/3.png](images/ecs/3.png)