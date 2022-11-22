# Advanced Storage

## AWS Snow Family

- AWS 내외부로의 마이그레이션을 위한 데이터 수집 및 프로세싱 능력을 가진 높은 보안성을 지닌 이동형 디바이스

### 데이터 마이그레이션

- Snowcone
- Snowball Edge
- Snow Mobile

### 엣지 컴퓨팅

- Snowcone
- Snowball Edge

## Snow Family를 이용한 데이터 마이그레이션

![images/advanced_storage/1.png](images/advanced_storage/1.png)

- 데이터 마이그레이션의 난점 (데이터 용량이 거대하면 할수록)
  - 제한된 접속시간
  - 제한된 대역폭
  - 네트워크에 소진되는 비용
  - 대역폭을 공유하기 때문에 최대의 효율을 낼 수 없음
  - 접속 안정성이 의문
- 스노우 패밀리는 오프라인 디바이스를 직접 보내주기 때문에, 직접 접속형태의 네트워크를 이용할 수 있음

## 다이어그램

### 일반적인 구성

![images/advanced_storage/2.png](images/advanced_storage/2.png)

### 스노우 패밀리

![images/advanced_storage/3.png](images/advanced_storage/3.png)

## Snowball Edge (data transfer)

![images/advanced_storage/4.png](images/advanced_storage/4.png)

- 물리적 데이터 이동 수단
  - TB, PB단위의 데이터를 AWS 내외부로 이동할 때 사용
- 네트워크를 통한 이동 시 비용 문제를 해결하기 위해 사용
- 과금 단위는 이 데이터 저장소를 이동하고 작업하는데 드는 비용단위로 처리됨
- 블록 스토리지로 S3과 호환되는 오브젝트 스토리지로 이루어져 있음
- Snowball Edge Storage 최적화 모델
  - 80TB의 HDD 용량으로 S3과 호환됨
- Snowball Edge Compute 최적화 모델
  - 42TB의 HDD 용량으로 S3과 호환됨
- 사용 사례
  - 대규모 데이터 마이그레이션, DR 등

## Snowcone

![images/advanced_storage/5.png](images/advanced_storage/5.png)

- 보다 작은 컴퓨팅 지원 모바일 장치
- 가벼움 (2.1kg)
- 엣지컴퓨팅, 데이터 마이그레이션 용도
- 8TB의 스토리지 지원
- 스노우볼을 운반하기 어려운 상황에 사용될 수 있음 (크기 문제)
- 배터리와 케이블을 직접 연결할 수 있음
- AWS로 오프라인 배송을 해도 되고, 직접 인터넷 연결을 하여 AWS DataSync로 동기화도 가능

## Snowmobile

![images/advanced_storage/6.png](images/advanced_storage/6.png)

- 엑사바이트 단위의 데이터 이동 수단 (1EB = 1000PB = 1000000TB)
- 하나의 스노우모빌이 100PB의 용량을 가지고 있음 (동시에 여러대도 연결 가능)
- 보안성
  - 온도 조절
  - GPS
  - 24/7 보안감시 카메라 등
- 10PB 이상의 데이터를 이동하려면 스노우볼보다는 스노우모빌이 더 좋은 선택지가 될 수 있음

## 스노우 패밀리 정리

![images/advanced_storage/7.png](images/advanced_storage/7.png)

## 스노우볼 이용 순서

1. 스노우볼 요청 (AWS 콘솔)
2. 스노우볼이 배송되면 스노우볼 클라이언트를 설치
3. 클라이언트를 이용해서 서버에서 데이터 수집
4. AWS로 스노우볼 반송
5. S3 버킷에 데이터가 저장됨
6. 스노우볼에 저장되어 있던 컨텐츠 영구삭제

## 엣지 컴퓨팅이란?

- 엣지 로케이션에서의 데이터 처리에 대한 전반적인 사항들
  - 길가에 트럭이나 해상 선박, 광산 채굴처 등이 엣지 로케이션

![images/advanced_storage/8.png](images/advanced_storage/8.png)

- 이러한 장소에서는
  - 인터넷 엑세스 불가능
  - 컴퓨팅 자원에 접근이 힘듬
- 그러한 곳에 스노우볼 엣지 스노우콘등을 이용하여 엣지 컴퓨팅을 실현 가능
- 사용 사례
  - 데이터 선처리
  - 머신 러닝
  - 미디어 스트림의 변환
- 결국 이러한 처리된 데이터는 AWS로 반환되어 보존될 필요가 있음

## 스노우 패밀리 엣지 컴퓨팅

- 스노우콘
  - 2 CPU, 4GB memory
  - 유무선 대응
  - USB-C 파워 공급
- 스노우볼 엣지 (컴퓨팅 최적화)
  - 52 vCPU, 208 GiB 메모리
  - GPU 옵션 (비디오 처리나 머신 러닝)
  - 42TB 가용 용량
- 스노우볼 엣지 (스토리지 최적화)
  - 40 vCPU, 80GiB 메모리
  - 오브젝트 스토리지 클러스터링
- EC2 인스턴스 기동 가능 AWS 람다 함수 실행 가능 (AWS IoT Greengrass)
- 장기 임대 가능 (1~3년 할인 모델)

## OpsHub

- 이전에는 스노우볼을 조작하기 위해 CLI가 제공되었음
- 최근에는 OpsHub를 지원하는데, 이는 노트북 등에 설치해서 스노우 패밀리와 연결하여 GUI 환경에서 AWS를 조작하듯 스노우 패밀리를 컨트롤 할 수 있는 대시보드를 제공하는 것
- 장치 모니터링
- EC2 등의 설치나 NFS 관리 등 다양한 컨트롤러 제공

![images/advanced_storage/9.png](images/advanced_storage/9.png)

## 스노우볼 to S3 글래시어

- 스노우볼에 담은 데이터는 글래시어로 바로 import 할 수 없음
- S3을 거쳐서 lifecycle policy를 통해 글래시어로 전환해야 함

![images/advanced_storage/10.png](images/advanced_storage/10.png)
