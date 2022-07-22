# Docker Image Compression (save and load)

- 전달해줄 환경이 인터넷이 불가능하여 파일 형태로 직접 `docker image`를 전달해야 할 경우 사용
- 인터넷이 불가능하면 `docker hub`의 이용이 불가능하기 때문에 이런 경우가 유스케이스가 됨

## 이미지 압축파일로 저장

- 이미지를 `tar` 압축파일로 저장

```bash
# docker save -o [OUTPUT-FILE] IMAGE
# ubuntu:focal 이미지를 ubuntu_focal.tar 압축 파일로 저장
$ docker save -o ubuntu_focal.tar ubuntu:focal
```

## 이미지 압축파일에서 불러오기

- 이미지를 `tar` 압축파일로부터 불러오기

```bash
# docker load -i [INPUT-FILE]
# ubuntu_focal.tar 압축 파일에서 ubuntu:focal 이미지 불러오기
$ docker load -i ubuntu_focal.tar
```

## 실습

```bash
$ docker save -o my-app-v2.tar my-app:v2
$ docker images
REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
my-app                        v2        21d6d723beae   26 hours ago    404MB
$ docker image rm my-app:v2
Untagged: my-app:v2
Deleted: sha256:21d6d723beaebc8fd9af9428974e730213b353b2e6428e77efa930066e2eface
$ docker load -i my-app-v2.tar
Loaded image: my-app:v2
```
