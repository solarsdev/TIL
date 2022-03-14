# 엘라스틱 빈스토크

- 엘라스틱 빈스토크는 AWS에서 어플리케이션을 배포함에 있어서 개발자 중심의 뷰를 제공한다.
- 엘라스틱 빈스토크에서는 지금껏 보아왔던 모든 도구들을 이용한다.
  - EC2
  - ASG (Auto Scaling Group)
  - ELB (Elastic Load Balancer)
  - RDS (Relational Database Service)
  - etc
- 그러나 이 모든것들은 하나의 뷰로 통합된다.
- 그럼에도 불구하고 모든 설정들은 전부 활용 가능하다.
- 빈스토크 자체는 무료이고, 사용된 리소스에 대해서만 비용을 지불하면 된다.

## Beanstalk

- 관리형 서비스
  - 인스턴스 설정 / OS는 빈스토크에 의해 관리된다.
  - 배포 전략은 설정할수 있지만, 엘라스틱 빈스토크에 의해 수행된다.
- 단지 코드를 업로드 하는것만이 개발자의 책임이다.
- 세가지 아키텍쳐 모델이 존재한다.
  - 하나의 인스턴스 배포 전략: 개발환경에 적합
  - LB + ASG: 프로덕션 환경 또는 pre-production 웹 어플리케이션 환경
  - ASG only: 웹이 아닌 어플리케이션 프로덕션 환경 (worker, message apps, etc..)
- 세가지 컴포넌트로 구성된다.
  - 어플리케이션
  - 어플리케이션 버전: 각각의 배포는 버전을 부여받는다.
  - 환경 이름 (dev, test, prod..): 자유로운 명명
- 어플리케이션 버전을 환경으로 배포하고, 해당 버전을 다음 어플리케이션 환경으로 진행시킨다. (패치)
- 전 버전으로의 롤백 기능을 지원한다.
- 환경의 모든 라이프사이클 관리 기능을 지원한다.

![images/beanstalk/1.png](images/beanstalk/1.png)

- 어떤 언어를 지원하는가?
  - Go
  - Java SE
  - Java with Tomcat
  - .NET on Windows Server with IIS
  - Node.js
  - PHP
  - Python
  - Ruby
  - Packer Builder
  - Single Container Docker
  - Multicontainer Docker
  - Preconfigured Docker
- 만약 지원하지 않더라도 커스터마이징 가능하다.

## 엘라스틱 빈스토크 배포 모드

- 싱글 인스턴스

![images/beanstalk/2.png](images/beanstalk/2.png)

- 로드밸런서가 포함된 고가용성

![images/beanstalk/3.png](images/beanstalk/3.png)

## 업데이트 시의 빈스토크 배포방식

- 한번에 모두(all at once): 새 버전을 모든 인스턴스에 동시에 배포합니다. 배포가 수행되는 동안 환경에 있는 모든 인스턴스가 잠시 서비스 중지됩니다.
- 롤링(Rolling): 새 버전을 배치로 배포합니다. 각 배치는 배포 단계 동안 서비스에서 제외되므로 배치에 있는 인스턴스의 수만큼 환경의 용량이 감소합니다.
- 추가 배치를 사용한 롤링(Rolling with additional batch): 새 버전을 배치로 배포하지만, 먼저 새로운 배치의 인스턴스를 시작하여 배포 프로세스 중에 모든 용량이 유지되도록 합니다.
- 변경 불가능(Immutable) - 변경 불가능 업데이트를 수행하여 새 버전을 새로운 인스턴스 그룹에 배포합니다.
- 트래픽 분할(Traffic splitting) - 새 버전을 새 인스턴스 그룹에 배포하고 수신되는 클라이언트 트래픽을 일시적으로 기존 애플리케이션 버전과 새 애플리케이션 버전 간에 분할합니다.

## 엘라스틱 빈스토크의 CLI

- 기본 CLI외에 EB cli를 설치하면 추가적인 명령어를 얻게 됨
  - eb create
  - eb status
  - eb health
  - eb events
  - eb logs
  - eb open
  - eb deploy
  - eb config
  - eb terminate
