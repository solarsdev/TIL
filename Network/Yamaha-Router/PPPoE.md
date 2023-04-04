# PPPoE

## 개요

- BRI등의 회선 인터페이스 경유로 LAN간 접속을 하는 경우 Point-to-Point Protocol (PPP, RFC1661)이 이용됨
  - PPP를 이더넷상에서 실현하는 것이 PPP over Ethernet(이하 PPPoE, RFC2516)
  - 이더넷을 통해 ISP에 접속하는데 사용됨
  - 후렛츠, ADSL을 이용하는 경우 PPPoE를 이용해서 ISP에 접속하게 됨
- RT에 PPPoE 설정을 하면 엑세스 컨센트레이터와의 대응 IP 패킷의 캡슐화는 RT에서 수행함
  - 랜측에서는 PPPoE 설정을 수행할 필요가 없어짐
- RT에 설정하는 config은 PP에 대해서 설정하게 됨
- PPPoE를 이용하는 랜 인터페이스의 명시 커맨드 (pppoe use)등 종래의 PP에 대한 설정과 기본적으로는 동일
- 단, MP와 압축기능은 사용 불가능함
- 또한 패킷 사이즈의 네고시에이션에 대한 설정이 필요
- connect/disconnect 커맨드에 의한 수동접속/절단도 가능
- 절단 타이머는 디폴트로는 무효로 되어있고, 한번 접속하면 자동으로 절단은 하지 않음
- 절단이 필요한 경우는 pppoe disconnect time 또는 pppoe auto disconnect를 설정해야 함
- 설정에 따라서는 PPPoE 패킷을 랜측에 송신하는 것도 가능하지만 그 경우에는 랜측과 WAN측이 동일 물리 세그먼트가 되어 WAN측에서 LAN측의 패킷을 관찰할수 없게 되므로 보안상 문제가 발생할 가능성이 있음
- 구현된 PPPoE기능은 클라이언트로서 동작하므로, 서버에 대해서 접속은 가능하지만 서버로서의 기능은 불가능함
