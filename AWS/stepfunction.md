# Step Function

- 스텝펑션은 하나의 상태 머신으로 워크플로우를 수행한다
  - 풀필먼트 주문, 데이터 처리
  - 웹 어플리케이션, 하지만 어떤 워크플로우도 가능
- JSON으로 작성
- 워크플로우를 비주얼하게 볼 수 있고, 실행도 가능하며, 이력도 남긴다
- SDK API 요청으로 워크플로우를 수행할수도 있고, API 게이트웨이나 이벤트 브릿지, 콘솔상에서 실행 가능하다

## Task State

- 상태 머신에서 작업을 수행
- AWS 서비스를 호출
  - 람다 함수를 호출 가능
  - AWS Batch 작업을 수행 가능
  - ECS 작업이나 완료시까지 대기도 가능
  - DynamoDB에 데이터를 삽입
  - SNS나 SQS에 메시지를 발행
  - 다른 Step function을 부르거나 다른 워크플로우를 수행
- Activity Task
  - EC2, ECS, 온프레미스
  - 액티비티는 스텝펑션에서 작업을 폴링한다
  - 수행된 이후 결과를 스텝펑션에 반환
    ![images/stepfunction/1.png](images/stepfunction/1.png)

## 스텝펑션의 상태

- Choice State : 조건을 검사하고 브랜치로 보냄 (혹은 기본 브랜치로)
- Fail or Succeed State : 수행을 종료하고 상태를 반환 (fail or succeed)
- Pass State : 인풋과 아웃풋에 관계없이 스킵 또는 정해진 데이터를 작업없이 주입한다
- Wait State : 딜레이를 제공하여 어떠한 시간이나 날짜까지 기다린다
- Map State : 각 단계를 증감치에 의해 동적으로 변화시킨다
- **Parallel State : 브랜치를 동시에 수행한다**

## 비주얼 스텝펑션

- 상태도를 이용하면 어디서 성공하고 실패했는지 명확하게 알 수 있다

![images/setpfunction/2.png](images/setpfunction/2.png)
