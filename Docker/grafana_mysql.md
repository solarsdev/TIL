# Grafana + MySQL 구성

## 참고자료

### Grafana 도커 가이드

[Run Grafana Docker image](https://grafana.com/docs/grafana/next/setup-grafana/installation/docker/)

### MySQL 도커 가이드

[Mysql - Official Image | Docker Hub](https://hub.docker.com/_/mysql)

## 1단계 : Grafana 구성하기

### 요구사항

- Grafana의 3000번 포트는 호스트의 3000번 포트와 바인딩
- Grafana의 설정파일인 grafana.ini는 호스트에서 주입 가능하도록 구성하고 읽기전용 설정
- Grafana의 로컬 데이터 저장 경로를 확인하여 도커 볼륨 마운트
- Grafana의 플러그인 추가 설치를 위한 환경변수 설정
- 로그 드라이버 옵션을 통해 로그 로테이팅

### 그라파나의 기본 데이터베이스

- SQLite (로컬 데이터 저장)

## 2단계 : Grafana + MySQL 구성하기

### 요구사항

- 1단계 요구사항 포함
- grafana.ini를 통해 database 설정을 sqlite에서 MySQL로 변경
- MySQL 컨테이너를 docker-compose에 db 서비스로 추가
- grafana 서비스가 db 서비스를 database로 연결하도록 구성
- MySQL의 로컬 데이터 저장 경로 확인하여 도커 볼륨 마운트
