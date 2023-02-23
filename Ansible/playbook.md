# 플레이북 (playbook)

## 앤서블 오브젝트 개념

### 플레이북 (playbook)

- YAML로 정의
- 순서대로 정렬된 플레이(작업 목록) 절차
- set of plays

### 플레이 (play)

- 작업 목록 (tasks)
- 특정 호스트 목록에 대하여 수행

### 작업 (task)

- 앤서블의 수행 단위
- 애드혹 명령어는 한 번에 단일 작업 수행

### 모듈 (module)

- 앤서블이 실행하는 코드 단위
- 작업에서 모듈을 호출함

### 콜렉션 (collection)

- 모듈의 집합

## 실제 수행

### 인벤토리

```bash
[amazon]
amazon1 ansible_host=18.183.156.22 ansible_user=ec2-user
amazon2 ansible_host=35.78.106.235 ansible_user=ec2-user

[ubuntu]
ubuntu1 ansible_host=ec2-13-231-54-228.ap-northeast-1.compute.amazonaws.com ansible_user=ubuntu
ubuntu2 ansible_host=ec2-54-168-221-85.ap-northeast-1.compute.amazonaws.com ansible_user=ubuntu

[linux:children]
amazon
ubuntu
```

### 플레이북 (install-nginx.yaml)

```bash
---

# play 1 (ubuntu 호스트 그룹을 대상으로 nginx를 설치
- name: Install Nginx on Ubuntu # 플레이 이름
  hosts: ubuntu # ubuntu 인벤토리 그룹의 호스트들을 대상
  become: true # 루트 권한으로 상승
  tasks: # 태스크 목록
  - name: "Install Nginx"
    apt: # 패키지 관리자 (debian 계열)
      name: nginx # 패키지 이름
      state: present # present는 nginx가 존재(인스톨)를 보장
      update_cache: true # apt update를 이용하여 캐시를 업데이트 함

  - name: "Ensure nginx service started"
    service: # 서비스 관리도구
      name: nginx # nginx 서비스
      state: started # 실행(started)을 보장

# play 2
- name: Install Nginx on Amazon Linux # 플레이 이름
  hosts: amazon # amazon 인벤토리 그룹의 호스트들을 대상
  become: true # 루트 권한으로 상승
  tasks: # 플레이 내의 작업 목록
  - name: "Enable Nginx repository provided by Amazon" # 첫번째 작업 이름
    command: "amazon-linux-extras enable nginx1" # 커맨드 모듈을 이용해서 amazon 리눅스에서 nginx1을 활성화 (nginx 설치 하기 위함)

  - name: "Install Nginx" # 두번째 작업 이름
    yum: # 패키지 관리자 (redhat 계열)
      name: nginx # 패키지 이름
      state: present # present는 nginx가 존쟤(인스톨)를 보장

  - name: "Ensure nginx service started" # 세번째 작업 이름
    service: # 서비스 관리도구
      name: nginx # 서비스명
      state: started # 실행(started)를 보장
```

### 실제 수행

```bash
ansible-playbook -i inventory install-nginx.yaml
```

### 결과

```bash
PLAY [Install Nginx on Ubuntu] ***********************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************
ok: [ubuntu2]
ok: [ubuntu1]

TASK [Install Nginx] *********************************************************************************************************************************************************
changed: [ubuntu1]
changed: [ubuntu2]

TASK [Ensure nginx service started] ******************************************************************************************************************************************
ok: [ubuntu1] #ok는 변경이 없는데, ubuntu는 nginx 설치 이후 자동으로 실행까지 해줌
ok: [ubuntu2]

PLAY [Install Nginx on Amazon Linux] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************
[WARNING]: Platform linux on host amazon2 is using the discovered Python interpreter at /usr/bin/python3.7, but future installation of another Python interpreter could
change the meaning of that path. See https://docs.ansible.com/ansible-core/2.14/reference_appendices/interpreter_discovery.html for more information.
ok: [amazon2]
[WARNING]: Platform linux on host amazon1 is using the discovered Python interpreter at /usr/bin/python3.7, but future installation of another Python interpreter could
change the meaning of that path. See https://docs.ansible.com/ansible-core/2.14/reference_appendices/interpreter_discovery.html for more information.
ok: [amazon1]

TASK [Enable Nginx repository provided by Amazon] ****************************************************************************************************************************
changed: [amazon2]
changed: [amazon1]

TASK [Install Nginx] *********************************************************************************************************************************************************
changed: [amazon1]
changed: [amazon2]

TASK [Ensure nginx service started] ******************************************************************************************************************************************
changed: [amazon1] # 서비스 실행이 되었기 때문에 (playbook이 실행을 했기 때문에 changed)
changed: [amazon2]

PLAY RECAP *******************************************************************************************************************************************************************
amazon1                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
amazon2                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu1                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu2                    : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### 멱등성을 체크하기 위해 한번 더 수행시 결과

```bash
PLAY [Install Nginx on Ubuntu] ***********************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************
ok: [ubuntu2]
ok: [ubuntu1]

TASK [Install Nginx] *********************************************************************************************************************************************************
ok: [ubuntu2]
ok: [ubuntu1]

TASK [Ensure nginx service started] ******************************************************************************************************************************************
ok: [ubuntu2]
ok: [ubuntu1]

PLAY [Install Nginx on Amazon Linux] *****************************************************************************************************************************************

TASK [Gathering Facts] *******************************************************************************************************************************************************
[WARNING]: Platform linux on host amazon2 is using the discovered Python interpreter at /usr/bin/python3.7, but future installation of another Python interpreter could
change the meaning of that path. See https://docs.ansible.com/ansible-core/2.14/reference_appendices/interpreter_discovery.html for more information.
ok: [amazon2]
[WARNING]: Platform linux on host amazon1 is using the discovered Python interpreter at /usr/bin/python3.7, but future installation of another Python interpreter could
change the meaning of that path. See https://docs.ansible.com/ansible-core/2.14/reference_appendices/interpreter_discovery.html for more information.
ok: [amazon1]

TASK [Enable Nginx repository provided by Amazon] ****************************************************************************************************************************
changed: [amazon1]
changed: [amazon2]

TASK [Install Nginx] *********************************************************************************************************************************************************
ok: [amazon2]
ok: [amazon1]

TASK [Ensure nginx service started] ******************************************************************************************************************************************
ok: [amazon1]
ok: [amazon2]

PLAY RECAP *******************************************************************************************************************************************************************
amazon1                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
amazon2                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu1                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu2                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

- command 모듈은 chagned가 되었는데, command 모듈의 멱등성을 보장하기 위한 방법은 추후 찾아보기
