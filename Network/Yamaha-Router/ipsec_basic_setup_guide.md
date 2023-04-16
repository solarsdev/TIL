# IPSec 기본 설정가이드

## 소개

- IPSec에 관련된 기본적인 설정을 망라했기 때문에, 상황에 따라 조금씩 변경은 필요하더라도 아래의 기초적인 설정 내용은 바뀌지 않기 때문에 한번은 읽어보는 것을 추천

![images/ipsec_basic_setup_guide/1.png](images/ipsec_basic_setup_guide/1.png)

- 참고: 프로바이더에서 부여받은 글로벌IP를 표현하기 위해 172.16.0.0/12 범위의 IP주소를 사용함
- 참고: Rev.6.02.16 펌웨어 상에서의 설정을 기준으로 설명하고 있기 때문에 그 이전의 명령어에 대해서는 다를 수도 있음
- 이 구성에서 요건은 다음과 같음
  - RT1은 고정 글로벌IP로 172.16.253.100을 가짐
  - RT2는 고정 글로벌IP로 172.17.254.16-31을 가짐
  - 192.168.0.0/24와 192.168.1.0/24의 단말은 인터넷에 접속할 수 있음
  - 192.168.0.0/24와 192.168.1.0/24의 단말은 IPSec로 상호 통신 가능

## 라우터의 IP주소 설정

- 먼저 라우터의 IP주소를 설정해야 하는데, 라우터의 IP주소는 상호간의 라우터를 식별하기 위한 IP주소가 됨
- RT1은 글로벌IP주소가 하나뿐이므로 그것이 라우터의 IP주소가 되지만 RT2는 글로벌IP주소가 복수 있으므로 그 중에서 한가지를 고르게 됨 (172.17.254.30을 선택했다고 가정)
- 자신측의 라우터 IP주소를 설정하기 위해서는 `ipsec ike local address` 명령어를, 상대방측 라우터의 IP주소를 설정하기 위해서는 `ipsec ike remote address` 명령어를 사용함
- RT1 설정
  ```
  # ipsec ike local address 1 172.16.253.100
  # ipsec ike remote address 1 172.17.254.30
  ```
- RT2 설정
  ```
  # ipsec ike local address 1 172.17.254.30
  # ipsec ike remote address 1 172.16.253.100
  ```
- 여기서 1이라는 매개변수는 복수의 VPN을 구성할 때 상대를 구별하기 위해 필요한 식별자 (첫번째 구성이라는 의미로 1을 사용)
- NAT를 사용할 경우에는 ipsec ike local address 명령어로 프라이빗주소를 설정하는 것이 가능하지만 이 경우에는 ipsec ike remote address는 반드시 글로벌IP주소를 설정해야 함 (추후 설명하겠지만 이 부분은 기억해둘것)

## IKE의 매개변수 설정

- 다음으로 IKE에 대한 매개변수를 설정해야 하는데, 설정해야할 커맨드는 많지만 필수는 하나인데 바로 사전 공유 키(pre-shared-key)라고 불리는 비밀번호임
- 비밀번호의 설정에는 `ipsec ike pre-shared-key` 명령어를 사용함
- RT1 설정
  ```
  # ipsec ike pre-shared-key 1 text password
  ```
- RT2 설정
  ```
  # ipsec ike pre-shared-key 1 text password
  ```
- 여기서 1이라는 매개변수는 아까의 명령과 동일하게 상대의 라우터를 식별하기 위한 번호임
- text는 텍스트 형식의 비밀번호임을 지정하는 것이며, password가 그 비밀번호인데, 최대 32문자까지 등록이 가능하기 때문에 실제로는 더 복잡하게 설정하는 것이 추천됨
- 비밀번호는 IPSec의 통신을 시작할 때 상대를 식별하기 위해 사용되므로 비밀번호의 설정은 양쪽의 라우터에서 동일한 것을 사용해야 함
- 다음으로는 필수는 아니지만 키의 유효기간(수명)을 설정할 수 있음
- IPSec에서는 키를 자동으로 생성하여 키의 수명이 다 되었을 경우, 새로운 키를 작성하여 바꿔주는데, 수명은 다음과 같이 설정 가능
- RT1 설정
  ```
  # ipsec ike duration isakmp-sa 1 24000
  # ipsec ike duration ipsec-sa 1 24000
  ```
- RT2 설정
  ```
  # ipsec ike duration isakmp-sa 1 24000
  # ipsec ike duration ipsec-sa 1 24000
  ```
- SA에는 SAKMP SA와 IPSec SA의 2종류가 있지만, 각각 다른 수명을 설정하는 것도 가능, 수명의 단위는 [초]
- 양쪽 라우터에 동일한 값을 설정하지 않아도 동작은 하지만 특별한 이유가 없는 이상 같은 값을 사용하는 것이 추천되며 이 명령어를 설정하지 않으면 28800초로 동작함

## 터널 인터페이스의 설정

- 터널 인터페이스는 상대 라우터와 연결하는 가상의 회선 인터페이스를 말함
- 터널 인터페이스에서 사용하는 암호의 방식을 선택해야 하는데, 방식에는 여러가지가 있지만 여기서는 ESP의 DES-CBC를 선정
- RT1 설정
  ```
  # ipsec sa policy 101 1 esp des-cbc
  ```
- 101은 단순한 등록번호로 1은 지금까지의 명령어들과 동일하게 상대 라우터의 식별번호임
- esp는 ESP를 가리키며, des-cbc는 DES-CBC를 가리킴
- 다음으로 아래 명령어를 통해 터널 인터페이스를 설정
- RT1 설정
  ```
  # tunnel select 1
  # ipsec tunnel 101
  # tunnel enable 1
  ```
- `tunnel select`와 `tunnel enable` 명령어는 1이라는 매개변수의 터널 인터페이스의 등록번호로 1이상의 정수를 지정해야 함
- `ipsec tunnel` 명령어는 아까 등록했던 `ipsec sa policy` 명령어의 등록번호를 지정함
- 마지막으로 터널 인터페이스에 대한 경로를 설정함 여기서는 스태틱 경로를 등록하는 것으로 함
- RT1 설정
  ```
  # ip route 192.168.1.0/24 gateway tunnel 1
  ```
- 이는 상대방 랜인 192.168.1.0/24로의 IP패킷을 정의한 터널 인터페이스로 송신하게 됨
- 여기까지 RT1의 설정을 설명했고, 다음은 RT2에서 설정을 구성할텐데, RT1과 거의 동일하지만 경로의 설정에서 네트워크의 주소 정도만 다르게 됨
- RT2 설정

  ```
  # ipsec sa policy 101 1 esp des-cbc

  # tunnel select 1
  # ipsec tunnel 101
  # tunnel enable 1

  # ip route 192.168.0.0/24 gateway tunnel 1
  ```

## 기타 설정 (RT1)

- IPSec에 대한 설정은 대부분 끝났으나 여기서 회선 설정을 추가하여 전체 설정을 살펴보자
- RT1 설정

  ```
  ; LAN

  ip lan1 address 192.168.0.1/24

  ; WAN

  line type bri1 l128
  pp select 1
  pp bind bri1
  ip pp address 172.16.253.100
  pp enable 1
  ip route default gateway pp 1

  ; IPsec

  ipsec ike local address 1 172.16.253.100
  ipsec ike remote address 1 172.17.254.30
  ipsec ike pre-shared-key 1 text himitsu

  ; TUNNEL

  ipsec sa policy 101 1 esp des-cbc
  tunnel select 1
  ipsec tunnel 101
  tunnel enable 1
  ip route 192.168.1.0/24 gateway tunnel 1
  ```

- 여기에 LAN의 단말이 인터넷에 접속할수 있도록, NAT 설정이 필요

  ```
  nat descriptor type 1 masquerade
  nat descriptor address outer 1 172.16.253.100
  nat descriptor address inner 1 172.16.253.100 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 172.16.253.100 udp 500
  nat descriptor masquerade static 1 2 172.16.253.100 esp *

  pp select 1
  ip pp nat descriptor 1
  pp enable 1
  ```

- 3행이 중요한 포인트로 NAT의 내부 주소로 라우터의 IP주소(172.16.253.100)을 포함하는 것에 주의할것
- NAT에는 외부 주소와 동시에 내부 주소도 있는데, 그 이유로는 단말과 동시에 라우터도 172.16.253.100을 사용하기 때문임 (RT1은 1개의 글로벌IP주소만 가지는것에 주의)
- 4행에서 IKE를 위해 UDP500번 포트를 라우터에 배당하는데, 이 설정으로 라우터가 IKE패킷을 수신할 수 있게 됨
- 그와 동시에 5행의 ESP를 라우터에 배당하여 라우터가 ESP패킷을 수신할 수 있게 됨
- NAT설정에서 주의해야 할 포인트는 아래처럼 4개의 명령어에 같은 IP를 설정하는 것
  ```
  ipsec ike local address 1 172.16.253.100
  nat descriptor address inner 1 172.16.253.100 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 172.16.253.100 udp 500
  nat descriptor masquerade static 1 2 172.16.253.100 esp *
  ```
- 이것만 지키면 라우터의 IP주소로 다른 주소를 선택하는 것 또한 가능한데, 예를 들면 LAN1 인터페이스의 주소인 192.168.0.1을 선택하여 아래처럼 설정을 변경하는 것도 가능
  ```
  ipsec ike local address 1 192.168.0.1
  nat descriptor address inner 1 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 192.168.0.1 udp 500
  nat descriptor masquerade static 1 2 192.168.0.1 esp *
  ```
- 어느쪽의 설정을 사용할지는 선호도 문제이지만, 전자의 설정에 비해 후자의 설정이 알기 쉬울수 있음
- RT1에서 ipsec ike local address 명령어로 프라이빗 주소를 설정한 경우에도 RT2에서 설정을 변경할 필요는 없음
- 이를 통해 양쪽 라우터에서의 주소가 반드시 일치하는 것은 아니라는 것을 알 수 있음
- RT1 설정
  ```
  # ipsec ike local address 1 192.168.0.1
  # ipsec ike remote address 1 172.17.254.30
  ```
- RT2 설정
  ```
  # ipsec ike local address 1 192.168.1.1
  # ipsec ike remote address 1 172.16.253.100
  ```

## 기타 설정 (RT2)

- 다음으로 RT2의 설정의 경우인데, 회선 설정을 추가한 경우
- RT2 설정

  ```
  ; LAN

  ip lan1 address 192.168.1.1/24

  ; WAN

  line type pri1 leased
  pri leased channel 1/1 1 24
  pp select 1
  pp bind pri1/1
  ip pp address 172.17.254.30
  pp enable 1
  ip route default gateway pp 1

  ; IPsec

  ipsec ike local address 1 172.17.254.30
  ipsec ike remote address 1 172.16.253.100

  ; TUNNEL

  ipsec sa policy 101 1 esp des-cbc
  tunnel select 1
  ipsec tunnel 101
  tunnel enable 1
  ip route 192.168.0.0/24 gateway tunnel 1
  ```

- 여기에 LAN의 단말들이 인터넷에 접속할 수 있도록 NAT의 설정을 추가한다

  ```
  nat descriptor type 1 nat-masquerade
  nat descriptor address outer 1 172.17.254.17-172.17.254.29
  nat descriptor address inner 1 192.168.1.1-192.168.1.254

  pp select 1
  ip pp nat descriptor 1
  pp enable 1
  ```

- RT1과 달리 글로벌IP주소가 여러개가 되므로 IPSec에서 사용하는 주소(172.17.254.30)을 라우터에 전용으로 설정함
- 따라서 RT1처럼 정적 IP 마스커레이드 설정은 필요하지 않음

# 기본 설정가이드

## 소개

- IPSec에 관련된 기본적인 설정을 망라했기 때문에, 상황에 따라 조금씩 변경은 필요하더라도 아래의 기초적인 설정 내용은 바뀌지 않기 때문에 한번은 읽어보는 것을 추천

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/78628f61-5691-4517-81f9-cdfa2b98437a/Untitled.png)

- 참고: 프로바이더에서 부여받은 글로벌IP를 표현하기 위해 172.16.0.0/12 범위의 IP주소를 사용함
- 참고: Rev.6.02.16 펌웨어 상에서의 설정을 기준으로 설명하고 있기 때문에 그 이전의 명령어에 대해서는 다를 수도 있음
- 이 구성에서 요건은 다음과 같음
  - RT1은 고정 글로벌IP로 172.16.253.100을 가짐
  - RT2는 고정 글로벌IP로 172.17.254.16-31을 가짐
  - 192.168.0.0/24와 192.168.1.0/24의 단말은 인터넷에 접속할 수 있음
  - 192.168.0.0/24와 192.168.1.0/24의 단말은 IPSec로 상호 통신 가능

## 라우터의 IP주소 설정

- 먼저 라우터의 IP주소를 설정해야 하는데, 라우터의 IP주소는 상호간의 라우터를 식별하기 위한 IP주소가 됨
- RT1은 글로벌IP주소가 하나뿐이므로 그것이 라우터의 IP주소가 되지만 RT2는 글로벌IP주소가 복수 있으므로 그 중에서 한가지를 고르게 됨 (172.17.254.30을 선택했다고 가정)
- 자신측의 라우터 IP주소를 설정하기 위해서는 `ipsec ike local address` 명령어를, 상대방측 라우터의 IP주소를 설정하기 위해서는 `ipsec ike remote address` 명령어를 사용함
- RT1 설정
  ```
  # ipsec ike local address 1 172.16.253.100
  # ipsec ike remote address 1 172.17.254.30
  ```
- RT2 설정
  ```
  # ipsec ike local address 1 172.17.254.30
  # ipsec ike remote address 1 172.16.253.100
  ```
- 여기서 1이라는 매개변수는 복수의 VPN을 구성할 때 상대를 구별하기 위해 필요한 식별자 (첫번째 구성이라는 의미로 1을 사용)
- NAT를 사용할 경우에는 ipsec ike local address 명령어로 프라이빗주소를 설정하는 것이 가능하지만 이 경우에는 ipsec ike remote address는 반드시 글로벌IP주소를 설정해야 함 (추후 설명하겠지만 이 부분은 기억해둘것)

## IKE의 매개변수 설정

- 다음으로 IKE에 대한 매개변수를 설정해야 하는데, 설정해야할 커맨드는 많지만 필수는 하나인데 바로 사전 공유 키(pre-shared-key)라고 불리는 비밀번호임
- 비밀번호의 설정에는 `ipsec ike pre-shared-key` 명령어를 사용함
- RT1 설정
  ```
  # ipsec ike pre-shared-key 1 text password
  ```
- RT2 설정
  ```
  # ipsec ike pre-shared-key 1 text password
  ```
- 여기서 1이라는 매개변수는 아까의 명령과 동일하게 상대의 라우터를 식별하기 위한 번호임
- text는 텍스트 형식의 비밀번호임을 지정하는 것이며, password가 그 비밀번호인데, 최대 32문자까지 등록이 가능하기 때문에 실제로는 더 복잡하게 설정하는 것이 추천됨
- 비밀번호는 IPSec의 통신을 시작할 때 상대를 식별하기 위해 사용되므로 비밀번호의 설정은 양쪽의 라우터에서 동일한 것을 사용해야 함
- 다음으로는 필수는 아니지만 키의 유효기간(수명)을 설정할 수 있음
- IPSec에서는 키를 자동으로 생성하여 키의 수명이 다 되었을 경우, 새로운 키를 작성하여 바꿔주는데, 수명은 다음과 같이 설정 가능
- RT1 설정
  ```
  # ipsec ike duration isakmp-sa 1 24000
  # ipsec ike duration ipsec-sa 1 24000
  ```
- RT2 설정
  ```
  # ipsec ike duration isakmp-sa 1 24000
  # ipsec ike duration ipsec-sa 1 24000
  ```
- SA에는 SAKMP SA와 IPSec SA의 2종류가 있지만, 각각 다른 수명을 설정하는 것도 가능, 수명의 단위는 [초]
- 양쪽 라우터에 동일한 값을 설정하지 않아도 동작은 하지만 특별한 이유가 없는 이상 같은 값을 사용하는 것이 추천되며 이 명령어를 설정하지 않으면 28800초로 동작함

## 터널 인터페이스의 설정

- 터널 인터페이스는 상대 라우터와 연결하는 가상의 회선 인터페이스를 말함
- 터널 인터페이스에서 사용하는 암호의 방식을 선택해야 하는데, 방식에는 여러가지가 있지만 여기서는 ESP의 DES-CBC를 선정
- RT1 설정
  ```
  # ipsec sa policy 101 1 esp des-cbc
  ```
- 101은 단순한 등록번호로 1은 지금까지의 명령어들과 동일하게 상대 라우터의 식별번호임
- esp는 ESP를 가리키며, des-cbc는 DES-CBC를 가리킴
- 다음으로 아래 명령어를 통해 터널 인터페이스를 설정
- RT1 설정
  ```
  # tunnel select 1
  # ipsec tunnel 101
  # tunnel enable 1
  ```
- `tunnel select`와 `tunnel enable` 명령어는 1이라는 매개변수의 터널 인터페이스의 등록번호로 1이상의 정수를 지정해야 함
- `ipsec tunnel` 명령어는 아까 등록했던 `ipsec sa policy` 명령어의 등록번호를 지정함
- 마지막으로 터널 인터페이스에 대한 경로를 설정함 여기서는 스태틱 경로를 등록하는 것으로 함
- RT1 설정
  ```
  # ip route 192.168.1.0/24 gateway tunnel 1
  ```
- 이는 상대방 랜인 192.168.1.0/24로의 IP패킷을 정의한 터널 인터페이스로 송신하게 됨
- 여기까지 RT1의 설정을 설명했고, 다음은 RT2에서 설정을 구성할텐데, RT1과 거의 동일하지만 경로의 설정에서 네트워크의 주소 정도만 다르게 됨
- RT2 설정

  ```
  # ipsec sa policy 101 1 esp des-cbc

  # tunnel select 1
  # ipsec tunnel 101
  # tunnel enable 1

  # ip route 192.168.0.0/24 gateway tunnel 1
  ```

## 기타 설정 (RT1)

- IPSec에 대한 설정은 대부분 끝났으나 여기서 회선 설정을 추가하여 전체 설정을 살펴보자
- RT1 설정

  ```
  ; LAN

  ip lan1 address 192.168.0.1/24

  ; WAN

  line type bri1 l128
  pp select 1
  pp bind bri1
  ip pp address 172.16.253.100
  pp enable 1
  ip route default gateway pp 1

  ; IPsec

  ipsec ike local address 1 172.16.253.100
  ipsec ike remote address 1 172.17.254.30
  ipsec ike pre-shared-key 1 text himitsu

  ; TUNNEL

  ipsec sa policy 101 1 esp des-cbc
  tunnel select 1
  ipsec tunnel 101
  tunnel enable 1
  ip route 192.168.1.0/24 gateway tunnel 1
  ```

- 여기에 LAN의 단말이 인터넷에 접속할수 있도록, NAT 설정이 필요

  ```
  nat descriptor type 1 masquerade
  nat descriptor address outer 1 172.16.253.100
  nat descriptor address inner 1 172.16.253.100 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 172.16.253.100 udp 500
  nat descriptor masquerade static 1 2 172.16.253.100 esp *

  pp select 1
  ip pp nat descriptor 1
  pp enable 1
  ```

- 3행이 중요한 포인트로 NAT의 내부 주소로 라우터의 IP주소(172.16.253.100)을 포함하는 것에 주의할것
- NAT에는 외부 주소와 동시에 내부 주소도 있는데, 그 이유로는 단말과 동시에 라우터도 172.16.253.100을 사용하기 때문임 (RT1은 1개의 글로벌IP주소만 가지는것에 주의)
- 4행에서 IKE를 위해 UDP500번 포트를 라우터에 배당하는데, 이 설정으로 라우터가 IKE패킷을 수신할 수 있게 됨
- 그와 동시에 5행의 ESP를 라우터에 배당하여 라우터가 ESP패킷을 수신할 수 있게 됨
- NAT설정에서 주의해야 할 포인트는 아래처럼 4개의 명령어에 같은 IP를 설정하는 것
  ```
  ipsec ike local address 1 172.16.253.100
  nat descriptor address inner 1 172.16.253.100 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 172.16.253.100 udp 500
  nat descriptor masquerade static 1 2 172.16.253.100 esp *
  ```
- 이것만 지키면 라우터의 IP주소로 다른 주소를 선택하는 것 또한 가능한데, 예를 들면 LAN1 인터페이스의 주소인 192.168.0.1을 선택하여 아래처럼 설정을 변경하는 것도 가능
  ```
  ipsec ike local address 1 192.168.0.1
  nat descriptor address inner 1 192.168.0.1-192.168.0.254
  nat descriptor masquerade static 1 1 192.168.0.1 udp 500
  nat descriptor masquerade static 1 2 192.168.0.1 esp *
  ```
- 어느쪽의 설정을 사용할지는 선호도 문제이지만, 전자의 설정에 비해 후자의 설정이 알기 쉬울수 있음
- RT1에서 ipsec ike local address 명령어로 프라이빗 주소를 설정한 경우에도 RT2에서 설정을 변경할 필요는 없음
- 이를 통해 양쪽 라우터에서의 주소가 반드시 일치하는 것은 아니라는 것을 알 수 있음
- RT1 설정
  ```
  # ipsec ike local address 1 192.168.0.1
  # ipsec ike remote address 1 172.17.254.30
  ```
- RT2 설정
  ```
  # ipsec ike local address 1 192.168.1.1
  # ipsec ike remote address 1 172.16.253.100
  ```

## 기타 설정 (RT2)

- 다음으로 RT2의 설정의 경우인데, 회선 설정을 추가한 경우
- RT2 설정

  ```
  ; LAN

  ip lan1 address 192.168.1.1/24

  ; WAN

  line type pri1 leased
  pri leased channel 1/1 1 24
  pp select 1
  pp bind pri1/1
  ip pp address 172.17.254.30
  pp enable 1
  ip route default gateway pp 1

  ; IPsec

  ipsec ike local address 1 172.17.254.30
  ipsec ike remote address 1 172.16.253.100

  ; TUNNEL

  ipsec sa policy 101 1 esp des-cbc
  tunnel select 1
  ipsec tunnel 101
  tunnel enable 1
  ip route 192.168.0.0/24 gateway tunnel 1
  ```

- 여기에 LAN의 단말들이 인터넷에 접속할 수 있도록 NAT의 설정을 추가한다

  ```
  nat descriptor type 1 nat-masquerade
  nat descriptor address outer 1 172.17.254.17-172.17.254.29
  nat descriptor address inner 1 192.168.1.1-192.168.1.254

  pp select 1
  ip pp nat descriptor 1
  pp enable 1
  ```

- RT1과 달리 글로벌IP주소가 여러개가 되므로 IPSec에서 사용하는 주소(172.17.254.30)을 라우터에 전용으로 설정함
- 따라서 RT1처럼 정적 IP 마스커레이드 설정은 필요하지 않음

## 동작 확인

- 여기까지 의도적으로 설명에서 제외했지만 IPSec을 동작시키기 위해서는 아래의 설정이 필요함
- RT1 설정
  ```
  # ipsec auto refresh on
  ```
- RT2 설정
  ```
  # ipsec auto refresh on
  ```
- 이것으로 문제가 없다면 IPSec의 통신이 가능해짐
- 현재 상태를 확인하기 위해서는 `show ipsec sa` 명령어를 실행

```
# show ipsec sa
SA[4] 寿命: 124秒
相手ホスト: 192.168.111.218, 送受信方向: 送受信
プロトコル: IKE
SPI: b9 2c 6f e9 56 4b 66 97 52 8f 0d e1 60 e2 33 95
鍵 : eb fc 81 25 04 ee b7 e8
----------------------------------------------------
SA[5] 寿命: 126秒
相手ホスト: 192.168.111.218, 送受信方向: 送信
プロトコル: ESP (モード: tunnel)
アルゴリズム: DES-CBC (認証: none)
SPI: b3 84 c6 02
鍵 : 85 78 23 e0 2f 89 37 49 4a 69 17 ac 88 92 df ca
----------------------------------------------------
```

- 이 실행처럼 SA의 정보가 표시된다면 정상적으로 동작하는 것임
