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

![images/high_availability_and_scailability/1.png](images/high_availability_and_scailability/1.png)

## 수평적 확장 (Horizontal Scalability)

- 수평적 확장은 시스템/어플리케이션의 복제로 양을 늘리는 것으로 설명됨
- 수평적 확장은 분산 가능한 시스템에만 적용 가능
- 웹 어플리케이션과 모던 어플리케이션에서 주로 사용되는 방식
- 퍼블릭 클라우드의 등장으로 수평적 확장이 매우 쉬워짐 (클릭 몇번 혹은 자동으로 인스턴스 증설 가능)

![images/high_availability_and_scailability/2.png](images/high_availability_and_scailability/2.png)

## 고가용성 (High Availability)

- 고가용성은 일반적으로는 수평적 확장으로 달성 가능
- 고가용성이란 어떤 어플리케이션/시스템이 2개 이상의 데이터 센터에서 동시 운영되고 있음을 의미 (데이터 센터는 AWS에서 AZ를 의미)
- 특정 데이터 센터에 장애가 생겨 전체를 사용할수 없게 되더라도 서비스가 유지되는 것
- AWS에서의 고가용성은 수동적 방식과 적극적 방식이 존재
  - 수동적 방식
    - RDS의 복수 AZ구성이나 EFS의 복수 AZ 구성처럼 사용자가 직접 관리하지 않아도 되는 방식
  - 적극적 방식
    - 서비스나 어플리케이션을 직접 EC2등에 탑재하여 복수로 구성하는 방식

![images/high_availability_and_scailability/3.png](images/high_availability_and_scailability/3.png)

## EC2에서의 고가용성과 확장성

- 수직적 확장
  - EC2에서는 단순히 EC2의 사이즈를 증가시키는 방식으로 수직적 확장을 달성 가능
  - t2.nano (0.5GiB RAM, 1 vCPU)
  - u-l2tbl.metal (12.3TiB RAM, 448 vCPUs)
- 수평적 확장
  - 인스턴스를 복수 작성하면 수평적 확장을 달성 가능
  - 이를 위해 ASG (Auto Scaling Group), LB (Load Balancer) 등을 지원함
