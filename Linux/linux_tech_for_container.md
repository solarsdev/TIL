# 컨테이너를 구성하는 리눅스 기술

## 리눅스 배포판

- 전 세계적으로 약 300여 가지의 선택지(리눅스 배포판)존재
  - 3개 주요 리눅스 계열로 정리됨
- Public Cloud 상에도 많은 배포판을 제공

### Debian 계열

- Debian, Ubuntu, Mint Linux 등
- 오픈소스이고 안정성에 초점
- Ubuntu
  - 사용성과 Long Term Support(LTS)을 목표(5년)
  - 다수의 소프트웨어 리포지토리에서 패키지 사용 가능, DPKG 기반, apt-get/apt
  - 서버와 데스크톱에서 널리 사용

### Red Hat / Fedora 계열

- Fedora, Red Hat Enterprise Linux, CentOS, Oracle Linux, Amazon Linux 등
- Fedora는 6개월마다 새로운 버전, CentOS는 좀 더 긴 릴리즈 사이클
- RHEL (Red Hat Enterprise Linux)
  - RPM 기반, yum or dnf 사용
  - 엔터프라이즈 서버 환경을 타겟
  - 긴 릴리즈 사이클 (10년)

### openSUSE 계열

- openSUSE, SUSE Linux Enterprise Server
- 오픈소스이고 안정성에 초점
- SLES (SUSE Linux Enterprise Server)
  - rpm 기반, zypper
  - 관리 목적으로 YaST 사용 가능
  - 안전한 패키지 관리

### 배포판 선택 기준

- 워크로드에 따라 적합한 배포판을 선택
- ex) 웹 서비스
  ![images/linux_tech_for_container/1.png](images/linux_tech_for_container/1.png)
- ex) SAP 워크로드
  - SUSE
- 엔터프라이즈
  - Red hat
  - SUSE
  - Amazon Linux 2
    ![images/linux_tech_for_container/2.png](images/linux_tech_for_container/2.png)

### 리눅스 커널

- 모놀리식 커널
  - 커널 문맥에서 대부분의 OS 코드를 처리
  - 자원 효율적이지만, 특정 커널 코드의 에러로 인한 영향이 커널 전체에 영향을 줌 (커널 패닉)
- 마이크로 커널
- 하이브리드 커널
  - 하이브리드 커널은 모놀리식 커널과 마이크로 커널의 장점을 결합한 형태로,
    일부 기능을 마이크로 커널화 하여 안정성과 확장성을 높이고 있다.
