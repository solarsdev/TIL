# BGP-4 사양

## BGP-4에 대해서

### BGP의 위치

- 현재 인터넷에서는 다양한 경로제어 프로토콜이 사용되고 있음
- 경로제어 프로토콜에는 각각의 특징이 있어, 다루는 네트워크의 규모에 따라 IGP, EGP라고 불리는 분류로 되어 있음
- IGP는 Interior Gateway Protocol의 약어로, 주로 조직내의 네트워크를 제어하는데 이용됨
  - 잘 알려진 IGP로는 RIP, OSPF, IS-IS등이 있음
- EGP는 Exterior Gateway Protocol의 약어로, IGP에서 운용되는 조직간을 소지함
  - EGP는 IGP에서 관리되는 네트워크를 연결하여 인터넷 전체의 접속성을 제공함
  - 잘 알려진 EGP로는 BGP가 있음
