# Docker volume

## Docker 레이어 아키텍쳐

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/985408a1-ccf5-40fe-b6e1-b01a87b7ddf8/Untitled.png)

### 이미지 레이어

```go
Layer 5: Update Entrypoint
Layer 4: Source code
Layer 3: Install in pip packages
Layer 2: Changes in apt packages
Layer 1: Base Ubuntu Layer
```

- docker build -t app .
- Dockerfile을 기반으로해서 이미지를 작성하게 됨
- 이미지는 레이어 구조로 되어 있음
- 명령어가 레이어로 쌓이듯이 저장되는 형태
- 이미지의 변경이 발생했을 경우 변경되기 전 레이어는 브랜치에서 바로 복사됨
- Read only

### 컨테이너 레이어

- Read Write
- 컨테이너 레이어는 컨테이너 종료시 삭제 (임시 데이터 저장소)
- 따라서 컨테이너에서 영구적인 데이터 저장을 하기 위해 볼륨을 활용해야 함

## 호스트 볼륨

- 호스트의 디렉토리를 컨테이너의 특정 경로에 마운트

```bash
# 호스트의 /opt/html 디렉토리를 Nginx의 웹 루트 디렉토리로 마운트
$ docker run -d \
	--name nginx \
	-v /opt/html:/usr/share/nginx/html \
	nginx
```

- -v 호스트 디렉토리:컨테이너 디렉토리

## 볼륨 컨테이너

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c002ac6-b5be-4230-b215-ffdddf2ddac8/Untitled.png)

- 특정 컨테이너의 볼륨 마운트를 공유

```bash
$ docker run -d \
	--name my-volume \
	-it \
	-v /opt/html:/usr/share/nginx/html \
	ubuntu:focal

# my-volume 컨테이너의 볼륨을 공유
$ docker run -d \
	--name nginx \
	--volumes-from my-volume \
	nginx
```

## Docker 볼륨

- 도커가 제공하는 볼륨 관리 기능을 활용하여 데이터를 보존
- 기본적으로 `/var/lib/docker/volumes/${volume-name}/_data` 에 데이터가 저장됨

```bash
# web-volume 도커 볼륨 생성
$ docker volume create --name db

# 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 마운트
$ docker run -d \
	--name fastcampus-mysql \
	-v db:/var/lib/mysql \
	-p 3306:3306 \
	mysql:5.7
```

## 읽기 전용 볼륨 연결

- 볼륨 연결 설정에 :ro 옵션을 통해 읽기 전용 마운트 옵션을 설정

```bash
# 도커의 web-volume 볼륨을 Nginx의 웹 루트 디렉토리로 읽기 전용 마운트
$ docker run -d \
	--name nginx \
	-v web-volume:/usr/share/nginx/html:ro \
	nginx
```
