# BGP-4 설정 가이드

## 전반적인 설정

- BGP-4를 이용하기 위해서는, `bgp use` 명령어를 설정할 필요가 있으며, 이 명령을 사용하지 않는 이상 BGP-4는 동작하지 않음에 유의할 것
  ```
  # bgp use on
  ```
- 다음으로 라우터가 소속될 AS번호를 설정해야 하는데, bgp autonomous-system 명령어를 사용함
  - 이 명령어로 설정 가능한 AS번호는 하나 뿐이며, 하나의 라우터는 복수의 AS에 소속될 수 없음
  ```
  # bgp autonomous-system 65480
  ```
- BGP에서는 라우터를 식별하기 위해 라우터ID라는 식별자를 송수신함
  - 라우터 ID에는 라우터 IP주소를 설정하며 설정된 IP주소는 실제로 라우터의 인터페이스에 설정된 것으로 해야 함
  - 명령어로 명시하지 않는 경우에는 아래의 인터페이스에 부여되어 있는 프라이머리 IP주소 중 자동으로 선택하여 라우터 ID로 사용됨
    - LAN인터페이스
    - LOOPBACK인터페이스
    - PP인터페이스
  - 단, 의도하지 않은 IP주소가 라우터 ID로 사용되는 것을 방지하기 위해 명시적으로 라우터 ID를 설정하는 것을 추천
  - OSPF와 BGP-4와의 병용의 경우 bgp router id 명령어나 ospf router id 명령어 중 한쪽이 설정됨

## 이웃 라우터 설정

- BGP-4에서 통신하는 상대 라우터를 정의해야 하는데, 이 때 `bgp neighbor` 명령어를 사용함
  ```
  # bgp neighbor 1 65500 192.168.0.2
  ```
  - 상기 예에서 IP주소가 192.168.0.2이고 65500 AS번호를 가진 라우터 사이에서 BGP-4 접속을 수행함
  - 매개변수 1은 이웃 라우터의 식별번호이며, 1 이상의 정수를 자유롭게 설정 가능
  - 피어링 통신에서 TCP MD5 인증을 이용하는 경우에는 `bgp neighbor pre-shared-key` 명령어를 사용
    ```
    # bgp neighbor pre-shared-key 1 text password
    ```
  - 양단 라우터에 같은 사전공유키를 설정해야 함

## 경로에 대한 필터링

- RIP이나 OSPF와 같은 BGP이외의 경로를 BGP-4에 광고할 경우에는 `bgp import filter` 명령어와 `bgp import` 명령어를 사용하여, 경로를 필터링하는 것이 가능
  ```
  # bgp import filter 1 equal 172.16.1.0/24
  # bgp import 65500 rip filter 1
  ```
- 또한, bgp export 명령어를 이용하여, BGP-4가 수신하는 경로에 대한 필터링을 거는 것이 가능함
  - 상기 명령어에서 정의한 경로만을 받아들여서 라우팅에 사용하거나 OSPF, RIP에 광고하는 것이 가능
  - bgp export 명령어의 설정예는 이하 표기
    ```
    # bgp export filter 1 equal 172.16.1.0/24
    # bgp export 65500 filter 1
    ```
  - 상기 예에서는 BGP-4에서 받은 172.16.1.0/24의 경로의 라우팅 테이블에 한해 받아들이게 됨
  - `bgp import filter` 명령어나 `bgp export filter` 명령어에서도 동일하지만, 네트워클 지정하지 않고 `all` 키워드를 사용하는 것이 가능한데, `all`은 `0.0.0.0/0`을 의미함
