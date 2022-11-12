# High Availability and Scalability

- 확장성이 의미하는 것은 어플리케이션 / 시스템이 더 큰 부하를 견딜 수 있도록 늘어나는 것
- 확장에는 두가지 방식이 있음
  - 수직적 확장
  - 수평적 확장 (유연성이라고도 부름)
- 확장성은 곧 가용성과 직결됨

## 수직적 확장 (Vertical Scalability)

- 수직적 확장은 인스턴스의 크기를 증가시키는 것으로 설명됨
- t2.micro를 t2.large로 변경하게 되면 그만큼 CPU 및 RAM의 사용량 증가를 커버할 수 있음
- 분산이 불가능한 시스템에서는 수직적 확장이 필요하고 데이터베이스 등이 그 예임
- 하드웨어 제약으로 인해 무한정 증가시키기는 힘든 방식

![images/high_availability_and_scalability/1.png](images/high_availability_and_scalability/1.png)

## 수평적 확장 (Horizontal Scalability)

- 수평적 확장은 시스템/어플리케이션의 복제로 양을 늘리는 것으로 설명됨
- 수평적 확장은 분산 가능한 시스템에만 적용 가능
- 웹 어플리케이션과 모던 어플리케이션에서 주로 사용되는 방식
- 퍼블릭 클라우드의 등장으로 수평적 확장이 매우 쉬워짐 (클릭 몇번 혹은 자동으로 인스턴스 증설 가능)

![images/high_availability_and_scalability/2.png](images/high_availability_and_scalability/2.png)

## 고가용성 (High Availability)

- 고가용성은 일반적으로는 수평적 확장으로 달성 가능
- 고가용성이란 어떤 어플리케이션/시스템이 2개 이상의 데이터 센터에서 동시 운영되고 있음을 의미 (데이터 센터는 AWS에서 AZ를 의미)
- 특정 데이터 센터에 장애가 생겨 전체를 사용할수 없게 되더라도 서비스가 유지되는 것
- AWS에서의 고가용성은 수동적 방식과 적극적 방식이 존재
  - 수동적 방식
    - RDS의 복수 AZ구성이나 EFS의 복수 AZ 구성처럼 사용자가 직접 관리하지 않아도 되는 방식
  - 적극적 방식
    - 서비스나 어플리케이션을 직접 EC2등에 탑재하여 복수로 구성하는 방식

![images/high_availability_and_scalability/3.png](images/high_availability_and_scalability/3.png)

## EC2에서의 고가용성과 확장성

- 수직적 확장
  - EC2에서는 단순히 EC2의 사이즈를 증가시키는 방식으로 수직적 확장을 달성 가능
  - t2.nano (0.5GiB RAM, 1 vCPU)
  - u-l2tbl.metal (12.3TiB RAM, 448 vCPUs)
- 수평적 확장
  - 인스턴스를 복수 작성하면 수평적 확장을 달성 가능
  - 이를 위해 ASG (Auto Scaling Group), LB (Load Balancer) 등을 지원함

## Load balancing이란?

- 로드 밸런싱은 이름 그대로 트래픽을 여러 서버로 분산시키는 것

![images/high_availability_and_scalability/4.png](images/high_availability_and_scalability/4.png)

## Load balancer를 사용하는 이유

- 부하를 여러 EC2 인스턴스로 분산할 수 있음
- 하나의 접속 포인트만 공개할 수 있음 (LB의 DNS)
- 다운스트림 인스턴스의 장애를 원활하게 해결 가능
- 서비스에 대한 모니터링을 수행 가능
- SSL Termination (HTTPS)을 수행 가능
- 쿠키에 의한 stickiness 기능 지원
- 복수 AZ에 대한 고가용성 지원
- 내부적 트래픽과 외부에서의 트래픽을 분리 가능

## AWS ELB (Elastic Load Balancer)

- ELB는 AWS의 관리형 로드 밸런서
  - AWS가 직접 관리하고 작동을 보장
  - 내부적 업그레이드 및 유지보수, 고가용성을 알아서 관리해줌
  - AWS상에서 정말 필요한 일부 설정만을 사용자에게 위임
- ELB는 사용하려면 비용이 발생하지만 실질적으로 그 비용 이상의 가치가 있음 (관리비용 삭감 등)
- AWS의 다른 많은 서비스들과 유기적으로 연계되는 기능을 제공
  - EC2, EC2 ASG, ECS
  - AWS Certificate Manager (ACM), CloudWatch
  - Route 53, AWS WAF, AWS Global Accelerator

## Health Check (동작감시)

- 동작감시는 LB의 핵심 기능중 하나
- 다운스트림의 인스턴스를 주기적으로 감시 (요청/응답)
- 하나의 포트와 경로를 통해 주기적으로 요청을 보냄
- 응답이 200이 아닌 경우 해당 다운스트림 객체를 비활성화 하여 트래픽을 보내지 않음

![images/high_availability_and_scalability/5.png](images/high_availability_and_scalability/5.png)

## AWS의 LB타입

- AWS에서는 4가지 타입의 로드밸런서를 제공
- Classic Load Balancer (v1) 2009년 CLB
  - HTTP, HTTPS, TCP, SSL (secure TCP)
- Application Load Balancer (v2) 2016년 ALB
  - HTTP, HTPS, WebSocket
- Network Load Balancer (v2) 2016년 NLB
  - TCP, UDP, TLS (secure TCP)
- Gateway Load Balancer 2020년 GLB
  - 3계층 동작 IP 프로토콜
- 기본적으로 새롭게 구성할때는 신버전의 로드밸런서를 사용하는것이 기능적으로 권장됨
- 특정 로드밸런서에는 internal과 external로 트래픽을 대응하는 방식이 다른 것들이 있음

## 로드밸런서의 보안그룹

![images/high_availability_and_scalability/6.png](images/high_availability_and_scalability/6.png)

## Classic Load Balancer (v1)

- TCP(레이어4), HTTP & HTTPS(레이어7)을 지원
- TCP 또는 HTTP 베이스로 동작감시 수행
- CLB는 별도의 호스트네임으로 제공됨 (xxx.region.elb.amazonaws.com)

![images/high_availability_and_scalability/7.png](images/high_availability_and_scalability/7.png)

## Application Load Balancer (v2)

- 7계층을 지원하는 어플리케이션 로드 밸런서 (HTTP)
- 로드 밸런서는 복수의 HTTP 서비스를 로드 밸런싱 할 수 있음 (타겟 그룹)
- 같은 머신 안에 있는 복수의 서비스 또한 로드 밸런싱 가능 (컨테이너형태)
- HTTP/2와 웹소켓 지원
- 리다이렉트 지원 (HTTP를 HTTPS로 변환 등)
- ALB는 마이크로서비스와 컨테이너 베이스 어플리케이션에도 적합하게 사용 가능
  - ECS 또는 도커 어플리케이션
- 포트 매핑을 지원하므로 같은 인스턴스 내에 다이나믹 포트로 운영되는 서비스도 지원가능하기 때문임
- 따라서, 클래식 로드 밸런서였다면 하나의 호스트만 지원하기 때문에 못하는 다양한 기능들을 ALB에서 제공함

### 타겟그룹

- URI 베이스로 타겟 그룹 지정 가능
  - example.com/users
  - example.com/posts
- 호스트명 기반으로 타겟 그룹 지정 가능
  - one.example.com
  - other.example.com
- 쿼리스트링 또는 헤더 기반으로 타겟 그룹 지정 가능
  - example.com/users?id=123&order=false
  - JWT

### HTTP Based Traffic 도식

![images/high_availability_and_scalability/8.png](images/high_availability_and_scalability/8.png)

## Target Groups

- EC2 인스턴스 (HTTP)
  - ASG등으로 자동화된 관리도 가능
- ECS 태스크 (HTTP)
  - ECS에서 관리되는 동적 컨테이너
- 람다 함수
  - 서버리스 함수형 서비스로, HTTP 요청이 JSON 이벤트로 변환되어 전달됨
- IP 주소
  - 내부 IP 주소 (S2S VPN등을 연결한 사내망 온프레미스등에 직접 연결 가능)
  - 단, 내부 IP 주소만 사용 가능
- ALB는 복수의 타겟 그룹을 운영 가능
- 동작감시는 타겟그룹단위로 수행되기 때문에 각각의 서비스를 별도로 확인하고 처리하게 됨

## 쿼리스트링 / 패러미터 라우팅

![images/high_availability_and_scalability/9.png](images/high_availability_and_scalability/9.png)

## ALB 추가정보

- ALB는 별도의 호스트네임으로 제공됨 (xxx.region.elb.amazonaws.com)
- 기본적으로 어플리케이션 서버에서는 클라이언트의 IP를 직접 알아낼 수는 없음
  - IP는 X-Forwarded-For 헤더명으로 기재됨
  - X-Forwarded-Port (포트), X-Forwarded-Proto (프로토콜) 또한 존재

![images/high_availability_and_scalability/10.png](images/high_availability_and_scalability/10.png)

- 클라이언트는 로드밸런서를 거쳐서 들어오기 때문에 서버측에서는 로드밸런서의 아이피로만 표시됨

## Network Load Balancer (v2)

- 레이어4 로드 밸런서
  - TCP & UDP 트래픽을 로드 밸런싱
  - 초당 백만단위의 요청을 처리 가능
  - ALB에 비해 더 낮은 지연 (ALB ~400ms NLB ~100ms)
- NLB는 AZ당 퍼블릭 IP를 1개씩 노출 가능함
  - EIP를 이용하면 IP 고정 가능
  - IP로만 소통 가능한 특정 서비스들과 연계시 사용 가능
- TCP 또는 UDP 트래픽에 대해 고속 통신이 필요한 경우 주로 사용됨

## NLB 트래픽 (TCP)

![images/high_availability_and_scalability/11.png](images/high_availability_and_scalability/11.png)

## NLB의 타겟그룹

- EC2 인스턴스
- IP 주소 (내부 주소)
- ALB
  - NLB와의 콤비네이션으로 IP주소 노출을 통한 엑세스 및 ALB의 HTTP 룰을 동시에 이용하기 위함
- NLB의 타겟그룹 동작감시는 TCP, HTTP, HTTPS 프로토콜로 진행됨

![images/high_availability_and_scalability/12.png](images/high_availability_and_scalability/12.png)

## Gateway Load Balancer

- AWS내에 서드파티 어플라이언스(방화벽, 패킷분석도구 등)를 설치해두고 사용하는 경우
- 게이트웨이 로드밸런서를 이용하여 어플라이언스 타겟 그룹을 생성하여 트래픽을 통과시킬 수 있음
- GLB는 레이어3(네트워크/IP패킷)에서 동작함
- Transparent Network Gateway
  - VPC내의 트래픽 경유지로 이용
- Load Balancer
  - 서드파티 어플라이언스에 트래픽 분산
- 내부적으로 GENEVE 프로토콜(포트 6081에서 동작)을 이용

![images/high_availability_and_scalability/13.png](images/high_availability_and_scalability/13.png)

### 타겟그룹

- EC2 인스턴스
- IP 주소 (온프레미스 머신)

![images/high_availability_and_scalability/14.png](images/high_availability_and_scalability/14.png)

## 스티키 세션 (Sticky Sessions)

- 스티키 세션을 적용하는 것으로 같은 유저를 항상 같은 백엔드 인스턴스로 연결시키는 것이 가능
- CLB 혹은 ALB에서 제공하는 기능
- 내부적으로 쿠키를 이용하여 유저를 같은 인스턴스로 배분
- 세션 데이터등을 백엔드 인스턴스에 직접 저장하는 서비스의 경우 활용 가능
- 스티키 세션을 적용하면 세션 데이터는 보존할 수 있지만, 유저가 특정 서버에 몰릴 수 있음

### 쿠키 이름

- Application-based Cookie (어플리케이션 기반 쿠키)
  - 커스텀 쿠키
    - 타겟 서비스가 생성할 수 있는 쿠키
    - 어플리케이션의 요구사항에 따라서 쿠키이름을 설정 가능
    - 쿠키 이름은 각 타겟 그룹마다 유니크하게 적용해야 함
    - 쿠키의 이름을 AWSALB, AWSALBAPP, AWSALBTG 등으로 사용하면 안됨 (AWS에서 예약어로 사용중)
  - 어플리케이션 쿠키
    - 로드밸런서에 의해 자동으로 만들어지는 쿠키
    - 쿠키 이름은 AWSALBAPP
- Duration-based Cookie (시간기반 쿠키)
  - 로드밸런서에 의해 자동으로 만들어지는 쿠키
  - 쿠키 이름은 ALB의 경우 AWSALB이며, CLB의 경우에는 AWSELB가 됨

![images/high_availability_and_scalability/15.png](images/high_availability_and_scalability/15.png)

## Cross-Zone Load Balancing (다중 존 로드밸런싱)

- 모든 AZ상에 있는 로드밸런서의 뒷단에 있는 백엔드 인스턴스의 트래픽을 균등하게 분배할 수 있는 기술
- 기본적으로 로드밸런서는 AZ마다 엔드포인트가 존재하며, 엔드포인트에 있는 백엔드 인스턴스에 균등하게 분배함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cbd0b719-0f59-48b7-8e54-bddb07390f95/Untitled.png)

- 상기 도식에서는 나와있지 않지만, 정확하게는 로드밸런서로 들어가는 트래픽이 분산되는것이 아닌, AZ1에 있는 로드밸런서로 이동했다 하더라도, 백엔드 인스턴스 전체를 균등하게 보고 나눠들어가는 방식
  - 따라서, 로드밸런서 AZ1에 이동했던 입구 트래픽을 AZ2에 있는 백엔드 인스턴스로 건네줄 경우가 생기는데, 이때 과금이 발생할수도 아닐수도 있음
- ALB
  - 이 옵션은 항상 켜져 있음 (비활성화 불가능)
  - 해당 기능으로 인해 AZ간 데이터 이동이 발생할경우 데이터 요금을 과금하지 않음
- NLB
  - 기본적으로 옵션이 비활성화 되어 있음
  - 활성화 하게 되면 AZ간 데이터 이동에 대한 과금 발생
- CLB
  - 기본적으로 옵션이 비활성화 되어 있음
  - 활성화 하더라도 AZ간 데이터 이동에 대한 과금 발생하지 않음

## SSL/TLS

- SSL 인증서는 클라이언트와 로드밸런서간 데이터 전송중 암호화를 제공
- SSL은 Secure Socket Layer의 약어로 접속 암호화에 이용됨
- TLS는 Transport Layer Security의 약어로 SSL의 새로운 버전
- 근래에는 SSL보다는 TLS가 기본적으로 많이 사용되나 그냥 SSL이라 불리고 있음
- 퍼블릭 SSL은 인증기관(CA)이 별로 존재하며 다양한 글로벌 인증기관이 있음
  - Comodo, Symantec, GoDaddy, Digicert, Letsencrypt, …
- SSL 인증서는 만료기간이 존재하며, 계속 사용하려면 반드시 연장이 필요함

## 로드밸런서의 SSL 인증서

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e389d51a-c69a-450f-8b28-b6978e121cf8/Untitled.png)

- 로드밸런서는 X.509 인증서를 이용 (SSL/TLS 서버 인증서)
- ACM(AWS Certificate Manager)을 이용하면 인증서 관리가 가능
- 타기관 발행된 인증서를 업로드하여 운용하는것도 지원됨
- HTTPS 리스너
  - 기본 인증서를 반드시 설정해야 함
  - 여러 도메인을 지원하기 위해 추가로 인증서를 선택하는것이 가능
  - SNI(Server Name Indication) 기능을 지원하기 때문에, 도메인이 여러개로 나눠져 있더라도 도메인에 맞는 인증서를 선택할 수 있음
  - SSL/TLS에는 다양한 버전이 있으며, 오래된 버전을 지원하기 위해 다양한 정책을 지원함

## SSL Server Name Indication

- SNI는 하나의 로드밸런서로 여러 타겟 그룹, 특히 다른 도메인을 사용하고 있는 경우 도메인을 식별하여 전송 중 암호화를 사용하기 위한 인증서를 선택하는 기능
- 새로운 버전의 프로토콜에서 지원되는 이러한 방식은 클라이언트가 대상 서버에 대해 접속을 요청할때 (초기단계의 핸드쉐이크에서) 어떠한 도메인에 접속하고자 하는지 인식하여 SSL 증명서를 결정하는것을 지원
- 서버측에서 올바른 인증서를 식별 가능하다면 올바르게 암호화되어 소통이 가능
- 이 기능은 CLB에서는 제공하지 않음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e4c5a75a-fd7a-4f00-b746-dd6726f53ff1/Untitled.png)

## Connection Draining (Deregistration Delay)

- 인스턴스가 어떤 이유로 인해 종료 예정이 되었을 경우 (스팟 인스턴스의 종료 요청, ASG에 의한 인스턴스 삭제 등) ELB는 자동으로 감지하여 인스턴스를 Draining 상태로 지정함
- ELB는 드레인 상태의 백엔드 인스턴스에 더이상 새로운 트래픽을 전송하지 않으며 기존 접속자들의 작업이 완료되기를 기다림
- 이러한 기능은 업로드와 같이 긴 시간이 요구되는 작업을 마무리할 시간을 제공하기 좋으며 설정 시간은 어플리케이션에서 어떤 종류의 연결을 필요로 하는지에 따라 달라짐
  - 설정 시간은 1초에서 3600초까지 가능 (디폴트는 300초)
  - 0으로 설정하면 드레인 기능이 완전히 정지됨
- 드레인 상태가 되면 접속이 불가능하지만 인스턴스가 존재하는것으로 인식되어 인스턴스의 교체 등이 일어나지 않기 때문에 최소한의 시간으로 설정하는 것이 좋음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1a21c53f-48d4-4f63-b950-8ee04b5f1880/Untitled.png)

## Auto Scaling Group

- 웹사이트의 부하량은 실시간으로 변화하고 있음
- 클라우드 환경을 사용하면 서버의 증설과 감설을 쉽게 대응할 수 있음
- ASG는 다음과 같은 목표롤 가지고 있음
  - 부하 증가시에 자동으로 규모를 증가 (EC2 인스턴스의 추가)
  - 부하 감소시에 자동으로 규모를 감소 (EC2 인스턴스의 제거)
  - 부하에 맞는 최소, 최대 단위의 인스턴스의 수량 조절
  - 인스턴스 추가시 자동으로 로드 밸런서에 대상으로 추가
  - 어떠한 이유로 인해 인스턴스에 오류가 발생했을 경우 새로운 대체 인스턴스로 교체
- ASG를 운용하는 것은 과금이 아님, ASG로 인해 발생하는 인스턴스에 대해서만 비용이 발생함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80ef434d-8753-46a0-a9d5-e685f8b03d43/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ccde61e3-549c-43a2-87cc-7240d3b6c385/Untitled.png)

### ASG 속성

- Launch Template (기동 템플릿)
  - AMI, Instance Type
  - EC2 User Data (부트스트래핑 코드)
  - EBS Volumes (볼륨)
  - Security Groups (보안그룹)
  - SSH Key Pair (키페어)
  - IAM Roles (IAM 역할)
  - Network 서브넷
  - 로드밸런서 정보
- 최소, 최대, 요구 수량
- 스케일 정책 설정

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/593c6432-5426-43a4-a75b-ae16feb39184/Untitled.png)

### ASG 클라우드워치 알람과 확장

- ASG에서는 클라우드워치 알람 기반으로 스케일링을 제공
- 알람은 특정 지표를 모니터링하는데, 예를 들면 CPU 사용량이나 기타 원하는 지표가 될 수 있음
- 지표의 기준점은 유저가 직접 설정 가능
- 알람을 기반으로
  - 스케일 아웃 확장 정책 (인스턴스의 숫자 증가)
  - 스케일 인 감소 정책 (인스턴스의 숫자 감소)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/85d1e500-7990-4575-9dab-5f7bfdde8c02/Untitled.png)

## ASG 스케일링 정책

### 다이나믹 스케일 정책

- 타겟 추적 스케일링
  - 가장 심플하고 설정하기 쉬움
  - 특정 지표를 기준으로 수치를 유지하도록 하는 것 (예를 들어, CPU 평균 사용량을 40%로 맞추기 등)
- 심플 / 스텝 스케일링
  - 클라우드워치 지표가 알람 상태로 들어갔을때 인스턴스 숫자를 어느정도 늘릴지 설정
  - 클라우드워치 지표가 정상으로 돌아왔을 때 인스턴스 숫자를 어느정도 감소시킬 지 설정
- 스케줄 액션
  - 특정 시간이 되었을때 정해진 작업을 수행
  - 예를 들면, 금요일 오후에 인스턴스 최소 숫자를 10에서 5로 감소시킴

### 예측 스케일링

- 어느정도 쌓인 데이터를 바탕으로 부하를 예측하여 스케일링을 시도

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/20711c5c-ae0d-43ae-9fb1-a523274bbd1f/Untitled.png)

- AWS의 머신러닝으로 제공되는 기능

## 스케일링 지표로 자주 사용되는 것들

- CPU 지표 (CPUUtilization)
  - 평균 CPU 사용량
- 타겟당 요청 수 (RequestCountPerTarget)
  - 평균적으로 타겟 당 어느정도의 요청이 들어가는 지
- 네트워크 In/Out
  - 어느정도의 네트워크 부하가 발생중인지
- 기타 지표
  - 어떤 지표든 타겟으로 삼아 설정할 수 있음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/285140b3-5d1b-407f-b231-159933655c66/Untitled.png)

## ASG 스케일 쿨다운

- 인스턴스가 스케일 활동으로 인해 상태가 변경되면 일종의 휴지기를 가짐 (기본 300초)
- 이 쿨다운 시간동안 ASG는 어떠한 지표가 변경되더라도 작동하지 않음
  - 인스턴스가 생성 혹은 삭제중일때 지표에 영향을 주는것을 최소화 하기 위함
- 만약 AMI를 전부 셋팅된 상태로 기본 설정값보다 빨리 안정되는 버전을 사용하고 있다면 300초가 생각보다 길게 느껴질 수 있으므로, 최소한도로 줄일 수 있음
  - 줄이게 되면 ASG가 다시 동작하는데 걸리는 시간이 줄어들기 때문에 바로 바로 인스턴스를 조절 가능

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2beb365f-0a69-4298-a3e9-a3e4c7ede967/Untitled.png)
