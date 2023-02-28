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
