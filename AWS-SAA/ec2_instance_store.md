# EC2 Instance Storage

## EBS 볼륨

![images/ec2_instance_store/1.png](images/ec2_instance_store/1.png)

- EBS(Elastic Block Storage)는 네트워크 볼륨으로 인스턴스가 실행중에 붙여서 사용할 수 있는 저장소
  - 네트워크 드라이브이기 때문에 인스턴스와의 소통이 약간의 지연이 발생할 수 있음
  - 하나의 인스턴스에서 다른 인스턴스로 이동시킬 수 있음
- EBS에 저장된 데이터는 영속적이며, 이는 인스턴스가 종료되어도 유지할 수 있음
  - 기본적으로는 루트 볼륨에 한해서 인스턴스가 삭제될 때 같이 삭제되도록 설정됨 (delete on terminate)
- EBS 볼륨은 동시에 하나의 인스턴스에만 사용가능
  - 단, 특정 EBS 타입의 경우에는 동시에 여러대의 인스턴스에서 사용 가능한 기능이 있음
- AZ에 종속적임
  - us-east1a에 생성된 EBS를 us-east1b에 있는 인스턴스에 붙일 수 없음
  - 스냅샷 기능을 통해 특정 AZ에서 다른 AZ로 복사가 가능하기 때문에 이런 기능을 이용해서 옮겨야 함
- 무료티어에 30GB의 EBS 볼륨에 대해 제공되고 있음
- 사전정의된 사양에 의해 할당받아 사용하는 개념
  - 용량과 IOPS등을 정의하고 해당 사양만큼 미리 할당받아 사용가능
  - 필요에 따라서는 사용중인 EBS의 용량의 증설이 가능함

## EBS 볼륨의 인스턴스 삭제시 동작

![images/ec2_instance_store/2.png](images/ec2_instance_store/2.png)

- 기본적으로 루트 볼륨은 Delete on Termination이 on인 상태로 추가됨
- 루트 볼륨이 아닌 볼륨은 Delete on Termination이 off인 상태로 추가됨
- 이 설정은 언제든 변경이 가능하기 때문에, 인스턴스가 제거되더라도 데이터를 남기고 싶을 경우에는 이를 활용하면 됨
