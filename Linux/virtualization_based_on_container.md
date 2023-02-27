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
