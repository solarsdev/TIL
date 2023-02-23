# 애드혹 명령어 (Ad-hoc Command)

## 앤서블을 사용하는 방법

- Ansible Playbook (yaml)
- ansible-pull
- Adhoc Command
    - 플레이북을 작성하지 않고, 직접 커맨드를 통해서 앤서블을 수행하는 방법

## 애드혹 사용법

```bash
# ansible host-pattern -m module [-a 'module options'] [-i inventory]
# 순서를 지킬 필요는 없음
ansible -i amazon.inv -m ping all # 해당 노드에 파이썬이 설치되어 앤서블 명령을 수행할 수 있는지 확인
# ansible --private-key -u 등을 이용하면 유저 명과 키 등을 직접지정 가능

ansible localhost -m setup
ansible -i vars.inv -m apt -a "name=git state=latest update_cache=yes" ubuntu --become
ansible -i vars.inv -m command -a "git --version" ubuntu
ansible -i vars.inv -m apt -a "name=git state=absent update_cache=yes" ubuntu --become

```

## 모듈

- ping 모듈이 하는 것 → icmp를 확인하는 것이 아닌, 대상 호스트에 접속 후 파이썬이 사용 가능한지 확인하는 것
    - 컨트롤 노드: 앤서블을 실행하는 데스크탑
    - 관리 노드: EC2에서 띄워진 VM (파이썬이 설치되어야 함)
