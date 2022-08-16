# `Grafana` + MySQL 구성

## 참고자료

### `Grafana` 도커 가이드

[Run Grafana Docker image](https://grafana.com/docs/grafana/next/setup-grafana/installation/docker/)

### `MySQL` 도커 가이드

[Mysql - Official Image | Docker Hub](https://hub.docker.com/_/mysql)

## 1단계 : `Grafana` 구성하기

### 요구사항

- `Grafana`의 3000번 포트는 호스트의 3000번 포트와 바인딩
- `Grafana`의 설정파일인 `grafana.ini`는 호스트에서 주입 가능하도록 구성하고 읽기전용 설정
- `Grafana`의 로컬 데이터 저장 경로를 확인하여 도커 볼륨 마운트
- `Grafana`의 플러그인 추가 설치를 위한 환경변수 설정
- 로그 드라이버 옵션을 통해 로그 로테이팅

### 그라파나의 기본 데이터베이스

- `SQLite` (로컬 데이터 저장)

### 도커 컴포즈 구성파일

```yaml
version: '3.9'

services:
  grafana:
    image: grafana/grafana:8.2.2
    restart: unless-stopped
    environment:
      GF_INSTALL_PLUGINS: grafana-clock-panel
    ports:
      - 3000:3000
    volumes:
      - ./files/grafana.ini:/etc/grafana/grafana.ini:ro
      - grafana-data:/var/lib/grafana
    logging:
      driver: 'json-file'
      options:
        max-size: '8m'
        max-file: '10'

volumes:
  grafana-data: {}
```

```
app_mode = production
instance_name = ${HOSTNAME}

#################################### Server ####################################
[server]
protocol = http
http_addr =
http_port = 3000

#################################### Database ####################################
; [database]
; type = mysql
; host = db:3306
; name = grafana
; user = grafana
; password = grafana

#################################### Logging ##########################
[log]
mode = console
level = info

#################################### Alerting ############################
[alerting]
enabled = true
```

- `restart` 옵션
  - `always`
    - 서버가 띄워져 있을때 컨테이너 내 오류 발생시 컨테이너 재시작
  - `unless-stopped`
    - 서버가 재시작 되더라도 도커 컨테이너를 다시 띄우기
- `environment`
  - `GF_INSTALL_PLUGINS`
    - 그라파나에서 제공하고 있는 도커 컨테이너 실행시 바로 플러그인 설치하는 환경변수 주입
- `port`
  - 호스트:컨테이너로 포트 바인딩
- `volumes`
  - `./files/grafana.ini:/etc/grafana/grafana.ini:ro`
    - 호스트에 있는 그라파나 설정 파일을 컨테이너 내부의 그라파나 설정파일로 마운트 + `Read Only`
  - `grafana-data:/var/lib/grafana`
    - 그라파나 데이터 볼륨을 컨테이너 내부의 그라파나 로컬 저장소에 마운트
- `logging`
  - `driver`
    - `json-file`은 도커에서 지정하는 기본으로 `json` 형식으로 로그를 출력함
  - `options`
    - `max-size`
    - `max-file`
      - 파일 로테이션 정책

## 2단계 : `Grafana` + `MySQL` 구성하기

### 요구사항

- 1단계 요구사항 포함
- `grafana.ini`를 통해 `database` 설정을 `SQLite`에서 `MySQL`로 변경
- `MySQL` 컨테이너를 `docker-compose`에 `db` 서비스로 추가
- `Grafana` 서비스가 `db` 서비스를 `database`로 연결하도록 구성
- `MySQL`의 로컬 데이터 저장 경로 확인하여 도커 볼륨 마운트

### 도커 컴포즈 구성파일

```yaml
version: '3.9'

services:
  db:
    image: mysql:5.7
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: grafana
      MYSQL_DATABASE: grafana
      MYSQL_USER: grafana
      MYSQL_PASSWORD: grafana
    volumes:
      - mysql-data:/var/lib/mysql
    logging:
      driver: 'json-file'
      options:
        max-size: '8m'
        max-file: '10'

  grafana:
    depends_on:
      - db
    image: grafana/grafana:8.2.2
    restart: unless-stopped
    environment:
      GF_INSTALL_PLUGINS: grafana-clock-panel
    ports:
      - 3000:3000
    volumes:
      - ./files/grafana.ini:/etc/grafana/grafana.ini:ro
      - grafana-data:/var/lib/grafana
    logging:
      driver: 'json-file'
      options:
        max-size: '8m'
        max-file: '10'

volumes:
  mysql-data: {}
  grafana-data: {}
```

```
app_mode = production
instance_name = ${HOSTNAME}

#################################### Server ####################################
[server]
protocol = http
http_addr =
http_port = 3000

#################################### Database ####################################
[database]
type = mysql
host = db:3306
name = grafana
user = grafana
password = grafana

#################################### Logging ##########################
[log]
mode = console
level = info

#################################### Alerting ############################
[alerting]
enabled = true
```

- `depends_on`
  - 특정 서비스가 먼저 실행된 이후에 해당 서비스를 실행하도록 함
- `grafana.ini`
  - `[database]`
    - `db:3306`
    - 도커 내에서 네임맵핑을 수행해주기 때문에 `docker-compose`에서 정의한 서비스명으로 지정 가능
