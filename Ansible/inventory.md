# 인벤토리 (Inventory)

- 대상서버 호스트를 관리하는 파일
- 그룹 기능을 지원
    - 한 호스트가 여러 호스트 그룹에 속할 수 있음
    - ex) 우분투 그룹에 특정 인스턴스를 속하게 함
    - ex) 현업에서는 프로덕션, 스테이징, 개발 환경등 환경 베이스로 구성 가능
- 인벤토리의 정의 방법
    - 스태틱 (static) → 실습에서 알아볼 인벤토리 방식
    - 다이나믹 (dynamic) → 클라우드를 이용하게 될 경우 ASG등으로 인해 아이피 정보 등이 빈번하게 교체될 경우

### 인벤토리

- 인벤토리는 확장자가 필요하지 않음
- 파일의 기재 형태만 맞으면 사용 가능
- 본인이 관리하기 편한 방식으로 파일명을 기재하면 됨

### Pattern 1

- 기본적으로는 아이피를 지정하게 됨

```bash
# amazon.inv
3.35.53.163
3.34.253.165
```

### Pattern 2

- 도메인을 사용할 수도 있음

```bash
# ubuntu.inv
ec2-3-34-49-77.ap-northeast-2.compute.amazonaws.com
ec2-13-209-20-108.ap-northeast-2.compute.amazonaws.com
```

### Pattern 3

- 그룹을 지정하는 방법

```bash
# amazon 그룹 내에 서버 2개가 IP로 지정되어 있음
[amazon]
3.35.53.163
3.34.253.165

# ubuntu 그룹 내에 서버 2개가 각각 도메인으로 지정되어 있음
[ubuntu]
ec2-3-34-49-77.ap-northeast-2.compute.amazonaws.com
ec2-13-209-20-108.ap-northeast-2.compute.amazonaws.com

# all 기본 그룹은 모든 호스트를 지정함
# localhost 그룹은 앤서블을 수행하는 관리노드를 의미
```

### Pattern 4

- 별칭 기능을 사용할 수 있음

```bash
# amazon 그룹 내에 amazon1, amazon2 별칭을 지정하고, 호스트에 대한 변수를 정의
# ansible_host는 ip또는 도메인이 올 수 있음
[amazon]
amazon1 ansible_host=3.35.53.163
amazon2 ansible_host=3.34.253.165

# ubuntu 그룹 내에 ubuntu1, ubuntu2를 도메인 형태로 호스트 지정
[ubuntu]
ubuntu1 ansible_host=ec2-3-34-49-77.ap-northeast-2.compute.amazonaws.com
ubuntu2 ansible_host=ec2-13-209-20-108.ap-northeast-2.compute.amazonaws.com
```

### Pattern 5

- ansible_user 변수 활용
- ansible에서는 ssh와 win_rm 프로토콜 기반으로 원격 명령을 수행
- 대상 서버의 로그인이 필요한데, 해당 로그인을 수행할 유저명을 지정할 수 있음
    - 도메인이나 아이피만 지정되어 있으면 whoami를 이용한 현재 사용자명을 이용하게 됨
    - whoami를 이용하고 싶지 않은 경우에 유저를 지정해야 함
- linux:children
    - linux 그룹은 다음에 명시된 children을 포함한다
    - “amazon, ubuntu 그룹은 linux 그룹의 하위 그룹이다” 임을 명시한 것

```bash
[amazon]
amazon1 ansible_host=3.35.53.163 ansible_user=ec2-user
amazon2 ansible_host=3.34.253.165 ansible_user=ec2-user

[ubuntu]
ubuntu1 ansible_host=ec2-3-34-49-77.ap-northeast-2.compute.amazonaws.com ansible_user=ubuntu
ubuntu2 ansible_host=ec2-13-209-20-108.ap-northeast-2.compute.amazonaws.com ansible_user=ubuntu

[linux:children]
amazon
ubuntu
```

### 추가적인 이해 필요

- Dynamic Inventory
- Group_vars
- Host_vars
