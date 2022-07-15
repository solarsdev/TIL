# Docker Entrypoint & command

## 엔트리 포인트와 커맨드

- [Entrypoint] [Command]

### 엔트리 포인트 (Entrypoint)

- 도커 컨테이너가 실행할 때 고정적으로 실행되는 스크립트 혹은 명령어
- 엔트리 포인트는 `생략할 수 있으며`, 생략될 경우 커맨드에 지정된 명령어로 수행

### 커맨드 (Command)

- 도커 컨테이너가 실행할 때 수행할 명령어 혹은 엔트리 포인트에 지정된 명령어에 대한 인자 값

## Dockerfile의 엔트리 포인트와 커맨드

```docker
FROM node:12-alpine
RUN apk add --no-cache python3 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production

ENTRYPOINT ["docker-entrypoint.sh"] # 엔트리포인트가 커맨드에 앞서 먼저 수행됨
CMD ["node"] # 기본적으로 실행될 명령어

[docker-entrypoint.sh](http://docker-entrypoint.sh) node
```

```bash
$ docker run --entrypoint sh ubuntu:focal
$ docker run --entrypoint echo ubuntu:focal hello world
```

## Docker 명령어의 엔트리 포인트와 커맨드

```bash
$ docker run ubuntu:focal
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND   CREATED         STATUS                     PORTS     NAMES
d82fde1ea518   ubuntu:focal   "bash"    9 seconds ago   Exited (0) 8 seconds ago             clever_hodgkin
$ docker inspect d82
...
"Config": {
    "Hostname": "d82fde1ea518",
    "Domainname": "",
    "User": "",
    "AttachStdin": false,
    "AttachStdout": true,
    "AttachStderr": true,
    "Tty": false,
    "OpenStdin": false,
    "StdinOnce": false,
    "Env": [
        "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    ],
    "Cmd": [
        "bash"
    ],
    "Image": "ubuntu:focal",
    "Volumes": null,
    "WorkingDir": "",
    "Entrypoint": null,
    "OnBuild": null,
    "Labels": {}
},
...
```

- inspect 결과의 Config란을 확인해보면 ubuntu:focal의 Entrypoint는 없고, Cmd는 bash이므로 기본 셸이 bash로 지정되어 있음을 알 수 있음

```bash
$ docker run --entrypoint sh ubuntu:focal
$ docker ps -a
CONTAINER ID   IMAGE          COMMAND   CREATED         STATUS                     PORTS     NAMES
cbf5d12ed25b   ubuntu:focal   "sh"      3 seconds ago   Exited (0) 3 seconds ago             distracted_tereshkova
d82fde1ea518   ubuntu:focal   "bash"    3 minutes ago   Exited (0) 3 minutes ago             clever_hodgkin

docker run --entrypoint sh -it ubuntu:focal                                                              ✔  13:08:18
# ls
bin  boot  dev	etc  home  lib	media  mnt  opt  proc  root  run  sbin	srv  sys  tmp  usr  var
#
```

- entry포인트를 sh로 지정할 경우 기존의 bash셸에서 sh셸로 변경된 것을 알 수 있음

<aside>
💡 Entrypoint와 Cmd는 컨테이너가 실행될때 수행되는 명령으로 이미지를 받아서 실행할때 입맛대로 맞게 실행하기 위해서 확인해야 하는 필수적인 항목들임

</aside>

[Dockerfile Entrypoint 와 CMD의 올바른 사용 방법](https://bluese05.tistory.com/77)
