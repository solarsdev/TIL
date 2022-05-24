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
