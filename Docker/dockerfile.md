# Dockerfile

## Dockerfile 문법

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

```docker
FROM node:12-alpine
RUN apk add --no-cache python3 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

## Format

```docker
# 사용법
# Comment
INSTRUCTION arguments

RUN echo 'we are running some # of cool things'
```

## Environment replacement

```docker
FROM busybox
ENV FOO=/bar
WORKDIR ${FOO}   # WORKDIR /bar *호스트가 아닌 컨테이너의 환경변수
ADD . $FOO       # ADD . /bar
COPY \$FOO /quux # COPY $FOO /quux
```

## ARG

- Docker build 시점에 사용할 인자를 미리 정의

<aside>
💡 It is not recommended to use build-time variables for passing secrets like github keys, user credentials etc. Build-time variable values are visible to any user of the image with the `docker history` command.

</aside>

<aside>
💡 빌드 시점에 깃허브 키나 유저 자격증명을 보내지 말것. 빌드 시점에 사용된 인자나 명령어는 이미지를 사용하는 모든 유저들이 `docker history` 명령어로 볼 수 있음

</aside>

```docker
# 사용법
ARG <name>[=<default value>]

FROM busybox
ARG user1
ARG buildno
# ...

$ docker build --build-arg CONT_IMG_VER=v2.0.1 .
```

## sample

```docker
#
# nodejs-server
#
# build:
#   docker build --force-rm -t nodejs-server .
# run:
#   docker run --rm -it --name nodejs-server nodejs-server
#

FROM node:16
# FROM -> 기본 이미지 이름
LABEL maintainer="FastCampus Park <fastcampus@fastcampus.com>"
LABEL description="Simple server with Node.js"
# LABEL -> 이미지의 메타데이터

# Create app directory
WORKDIR /app
# WORKDIR -> CWD를 지정함 (=cd)

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./
# COPY -> SRC DEST 형식으로 HOST IMAGE (명령어가 수행된 cwd) (WORKDIR에서 지정된 이미지내 경로)

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .
# 소스코드의 변경으로 인해 패키지 인스톨 레이어가 영향받지 않게 하기 위해 소스코드 카피는 뒤에 실행함

EXPOSE 8080
# 문서화 목적 (8080 포트를 사용함을 이미지 사용자에게 알려주기 위함)
CMD [ "node", "server.js" ]
```

## Entrypoint

```docker
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```

## ADD

- COPY와 거의 동일
- URL을 인자로 사용 가능하지만 변경확인이 어렵기 때문에 일반적으로 COPY를 사용
