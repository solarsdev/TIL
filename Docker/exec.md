# Docker exec

## 명령어 실행

- 실행중인 컨테이너에 명령어를 실행

```bash
$ docker exec [container] [command]

# my-nginx 컨테이너에 Bash 셸로 접속하기
$ docker exec -it my-nginx bash

# my-nginx 컨테이너의 환경변수 확인하기
$ docker exec my-nginx env
```

```bash
$ docker run -dp 80:80 --name my-nginx nginx                                                               ✔  13:44:55
bfebf520e0b8a678a3f61dde3fb1c96ae8dba20322779fcf05782e3d9a3355d6
$ docker exec my-nginx env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=bfebf520e0b8
NGINX_VERSION=1.23.0
NJS_VERSION=0.7.5
PKG_RELEASE=1~bullseye
HOME=/root
```
