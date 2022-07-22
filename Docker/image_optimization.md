# Docker Image Optimization

## 꼭 필요한 패키지 및 파일만 추가

- 일반 호스트 리눅스처럼 당장 사용하지 않을 패키지들을 설치하는것은 실수임
  - `vim`
  - `git`
  - `curl`
  - 기타 등등
- 최대한 필요한 패키지만 포함하는것이 모범 사례

## 컨테이너 레이어 수 줄이기

- 도커 파일 내에 지시어 수를 줄일수록 레이어가 줄어듦
- 예를 들어 `RUN`이 여러번 등장하면 각각의 `RUN`이 복수의 레이어로 발생

### 샘플코드에서 활용한 경량화 전략1

- 하나의 `RUN` 지시어에 `&&` 연결 명령어를 통해 복수 명령 수행하기
- `apk add` 명령 이용시에 캐시를 사용하지 않도록 `—no-cache` 옵션 부여하기
  - 도커 컨테이너에서는 설치시 캐시를 남겨둘 이유가 없기 때문에 (재설치가 필요 없기 때문에) 노캐시 옵션을 확인

```bash
#
# slacktee
#
# build:
#   docker build --force-rm -t slacktee .
# run:
#   docker run --rm -it --name slacktee slacktee
#

FROM alpine:3.14
LABEL maintainer="FastCampus Park <fastcampus@fastcampus.com>"
LABEL description="Simple utility to send slack message easily."

# Install needed packages
RUN \
  apk add --no-cache bash curl git && \
  git clone https://github.com/course-hero/slacktee /slacktee && \
  apk del --no-cache git
RUN chmod 755 /slacktee/slacktee.sh

# Run
WORKDIR /slacktee
ENTRYPOINT ["/bin/bash", "-c", "./slacktee.sh"]
```

## 경량 베이스 이미지 선택

- 베이스 이미지가 경량화된 것을 사용
  - `debian-slim`
  - `alpine`
  - s`tretch` (파일시스템만 포함되어 있음, `golang`에서 바이너리를 작성한 뒤에 사용)

### 샘플코드에서 활용한 경량화 전략2

```bash
REPOSITORY                    TAG         IMAGE ID       CREATED         SIZE
node                          16-alpine   463370be2c82   3 days ago      111MB
node                          16-slim     0f441aba9263   9 days ago      169MB
node                          16          747c5d561cc9   9 days ago      853MB
```

## 멀티 스테이지 빌드 사용

- 빌드에서 필요한 의존성, 릴리즈에 필요한 의존성을 나누기
- `FROM` 지시어에서 활용한 이미지를 `AS` 지시어로 임시 이미지 이름을 부여 (이렇게 하면 `FROM`을 여러번 사용 가능)
- `COPY package*.json ./` 까지는 동일 레이어를 사용하게 되며, `build`와 `release` 이후에 사용된 지시어들만 레이어로 추가됨

### 멀티스테이지 빌드시 용량이 줄어드는 이유

- 궁극적으로 왜 용량이 줄어드는가 → `npm install` 등 의존성 설치나 `npm build` 등 빌드 단계에서만 필요한 파일들이 또 존재한다는 이야기
- 실제 운영할 이미지에는 해당 파일들조차도 제거해서 정말 필요한 바이너리 파일만 이미지화 시키는 개념

```docker
#
# nodejs-server
#
# build:
#   docker build --force-rm -t nodejs-server .
# run:
#   docker run --rm -it --name nodejs-server nodejs-server
#

FROM node:16-alpine AS base
LABEL maintainer="FastCampus Park <fastcampus@fastcampus.com>"
LABEL description="Simple server with Node.js"

# Create app directory
WORKDIR /app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

FROM base AS build
RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

FROM base AS release
COPY --from=build /app/node_modules ./node_modules
# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
```

### 샘플코드 : `golang`

- `go`언어는 컴파일 언어라서 이해가 빠름 (컴파일 후 나온 바이너리만 복사)

```docker
# BUILDER
FROM golang:latest AS builder
WORKDIR /go/src/app
COPY . .

RUN go get -d -v ./...
RUN go build -o dalfox

# RUNNING
FROM debian:buster
RUN mkdir /app
COPY --from=builder /go/src/app/dalfox /app/dalfox
COPY --from=builder /go/src/app/samples /app/samples
WORKDIR /app/
CMD ["/app/dalfox"]
```
