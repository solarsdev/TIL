# 컨테이너 기반 가상화 소개

## VM과 컨테이너 비교

### 가상 머신 (Virtual Machine)

![images/virtualization_based_on_container/1.png](images/virtualization_based_on_container/1.png)

- 저수준 하드웨어 장치 (CPU, Disk, Network)를 가상화

### 장점

- 같은 서버(호스트OS)에 다양한 운영체제를 실행
- 물리머신 대비 동일한 자원을 더 효율적으로 사용
- 물리 머신 대비 빠른 서버 프로비저닝

### 단점

- OS 이미지, 라이브러리, 어플리케이션을 반복적으로 포함 (중복 설치 문제)

### 컨테이너 (Container)

![images/virtualization_based_on_container/2.png](images/virtualization_based_on_container/2.png)

- 어플리케이션 구동에 필요한 모든 종속성을 포함한 소프트웨어 패키지를 운영체제 위에서 가상화

### 장점

- 컨테이너를 어느 환경에나 배포 가능
- OS를 부팅하거나 라이브러리를 로드할 필요가 없음
- 가상 환경을 더 효율적이고 경량으로 생성 가능
- 수초 이내의 빠른 시작 시간
- 하나의 호스트에 더 많은 어플리케이션 실행 가능
- OS 패치 업데이트 등 유지 관리와 관련된 오버헤드 감소

### 단점

- 컨테이너가 정의된 운영체제에 종속성을 가짐

## 다중 운영체제 지원

- Docker에 다중 운영체제 사용이 가능할까?

![images/virtualization_based_on_container/3.png](images/virtualization_based_on_container/3.png)

- Docker Hub에 다양한 리눅스 배포판이 존재

```bash
$ cat /etc/os-release
NAME="Ubuntu"
...

$ docker pull fedora
$ docker run --rm fedora cat /etc/os-release
NAME="Fedora Linux"
...
```

- ‘Docker는 OS레벨 가상화 기술’ from Wiki
- OS 레벨 가상화란?
  - 커널이 여러 격리된 사용자 공간 인스턴스의 존재를 허용하는 운영체제의 패러다임
  - containers: LXC, Solaris containers, Docker, Podman
    zones: Solaris containers
    virtual kernels: DragonFly BSD
    jails: FreeBSD jail, chroot jail
  - 호스트 OS의 커널!을 공유
    ```bash
    $ uname -a
    > Linux 5.4.0-104-generic ~~
    $ docker run --rm fedora uname -a
    > Linux 5.4.0-104-generic ~~
    ```
- 리눅스와 리눅스 패포판의 차이
  - Ubuntu, Redhat, SUSE, CentOS (리눅스 패포판)
  - 리눅스 배포판 = 리눅스 커널 + 컴포넌트 (e.g. 윈도우 시스템, 데스크톱 환경, 서비스 데몬, 패키지 매니저, 어플리케이션 등)
- 리눅스와 리눅스 커널의 차이
  - 리눅스 커널: 하드웨어 자원을 관리하고 추상화하여 프로세스에게 할당하고 관리하는 역할을 수행

![images/virtualization_based_on_container/4.png](images/virtualization_based_on_container/4.png)

- 예를 들어, Ubuntu 호스트 Docker에 Amazon 리눅스 배포판을 실행
  - 리눅스 커널 + 컴포넌트 (e.g. ~~윈도우 시스템, 데스크톱 환경, 서비스 데몬~~, 패키지 매니저, 어플리케이션 등)
    ![images/virtualization_based_on_container/5.png](images/virtualization_based_on_container/5.png)
- 리눅스 도커에 윈도우 컨테이너를 올릴 수 있을까?
  - 불가, 커널을 공유하는 구조로는 윈도우를 리눅스 커널에 올릴 수 없음
- 그럼 윈도우나 맥호스트에 우분투 컨테이너가 동작하는가?
  - 가능, 단, 가상화 환경 (LinuxKit) 위에서 동작하기 때문에, 리눅스 컨테이너는 리눅스에서 동작시키는 것이 바람직하다.
