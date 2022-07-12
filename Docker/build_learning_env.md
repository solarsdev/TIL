# Docker & docker-compose 설치

- 기본적으로 Docker는 리눅스의 가상화 기술을 활용한 시스템이기 때문에 호스트 OS는 왠만하면 리눅스 계열을 선정하는 것이 좋음
- Windows의 WSL이나 MacOS에서는 완벽하게 호환이 되지 않을 가능성이 있음
- MacOS에서는 기본적으로 유닉스 기반이기 때문에 윈도우에서보다는 다소 나음

## Docker 설치

- Ubuntu 버전
  [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

### 설치 순서

```bash
#!/usr/bin/env bash
## INFO: https://docs.docker.com/engine/install/ubuntu/

set -euf -o pipefail

DOCKER_USER=ubuntu

# Install dependencies
sudo apt-get update && sudo apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

# Add Docker’s official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --yes --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker CE
sudo apt-get update && sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Use Docker without root
sudo usermod -aG docker $DOCKER_USER
```

## docker-compose 설치

[Install Docker Compose](https://docs.docker.com/compose/install/)

### 설치 순서

```bash
#!/usr/bin/env bash
## INFO: https://docs.docker.com/compose/install/

set -euf -o pipefail

DOCKER_COMPOSE_VERSION=v2.1.1

# Download and install
sudo curl -L "https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## Docker for Desktop 설치 (MacOS)

- 윈도우, 맥 운영체제에서 도커를 사용할 수 있도록 추가적인 경량 가상화 기술을 사용
- docker, docker-compose가 포함되어 있음
- 데스크탑 GUI 제공

### 설치 순서

```bash
brew install --cask docker
```

- Docker 데스크탑 실행

```bash
docker ps
```

# kubectl & kustomize 설치

## kubectl이란?

- 쿠버네티스 API와 통신하며 사용자 명령을 전달하는 CLI 툴

![https://raw.githubusercontent.com/sangam14/kubernets101/master/pic.svg?sanitize=true](https://raw.githubusercontent.com/sangam14/kubernets101/master/pic.svg?sanitize=true)

![https://raw.githubusercontent.com/sangam14/kubernets101/master/pic1.svg?sanitize=true](https://raw.githubusercontent.com/sangam14/kubernets101/master/pic1.svg?sanitize=true)

- 쿠버네티스에는 API 서버가 존재하고, API서버에 명령을 내리게 되는데 이를 관리할 수 있음

### 설치 방법

```bash
#!/usr/bin/env bash
## INFO: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management

set -euf -o pipefail

# Install dependencies
sudo apt-get update && sudo apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

# Add kubectl's official GPG key
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/kubernetes-archive-keyring.gpg

# Set up the repository
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# Install kubectl
sudo apt-get update && sudo apt-get install -y kubectl
```

## kustomize란?

- kustomize는 쿠버네티스의 매니페스트 파일을 좀 더 효율적으로 관리할 수 있도록 도와주는 툴
- 쿠버네티스는 클러스터를 관리해주는 오케스트레이션 시스템인데, 어플리케이션의 정보를 쿠버네티스 매니페스트라는 형태로 관리하게 됨
- kubectl을 이용해서 매니페스트를 관리할 수 있지만, 어플리케이션의 수가 늘어날수록 관리에 어려움을 겪게 됨
- 이를 돕기 위해 여러가지 툴이 등장하게 됨
  - kustomize → kubectl에 내장됨
  - helm → OSS에서 만들어져서 활발하게 개발되는 중
    - chart 패키징 기반
- 쿠버네티스 매니페스트 구조
  - base
  - patch, overlay
  - 베이스에 패치를 해서 여러가지 환경을 만들어냄
- 예를 들어 base값이 되는 어플리케이션 기본 정보가 있을때, 스테이징용, 개발용, 프로덕션용으로 추가적인 정보가 필요할 경우 해당 정보들이 patch가 됨

### 설치 방법

```bash
#!/usr/bin/env bash
## INFO: https://kubectl.docs.kubernetes.io/installation/kustomize/binaries/

set -euf -o pipefail

KUSTOMIZE_VERSION=v4.4.1

# Download kustomize binary
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/kustomize/${KUSTOMIZE_VERSION}/hack/install_kustomize.sh"  | bash

# Install to /usr/local/bin
sudo install -o root -g root -m 0755 kustomize /usr/local/bin/kustomize
```

# minikube 설치

## minikube

- 가상환경을 이용하여 쿠버네티스 클러스터를 구현
- 드라이버를 선택하여 원하는 가상환경 (docker, podman, virtualbox, parallels, vmware, hyperkit 등)에서 구성 가능
- 쿠버네티스 학습환경으로 활용하기 좋음
- 하나의 머신이 아니라 여러 머신을 클러스터라고 부르는데, 쿠버네티스를 실질적으로 운영환경에 맞게 구축하기 위해서는 여러대의 쿠버네티스 시스템을 갖추게 됨
- 학습을 위해서 여러대의 쿠버네티스 시스템을 구성하는것은 비용효율적이지 못함
- 심플한 쿠버네티스를 구성할수 있도록 minikube등이 등장했음

[minikube start](https://minikube.sigs.k8s.io/docs/start/)

### 요구사항

- 2CPUs or more
- 2GB of free memory
- 20GB of free disk space

### 설치 방법

```bash
#!/usr/bin/env bash
## INFO: https://minikube.sigs.k8s.io/docs/start/

set -euf -o pipefail

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

```bash
$ minikube start --driver docker
[출력내용 생략]
$ cat ~/.kube/config
apiVersion: v1
clusters: # 클러스터 정보
- cluster:
    certificate-authority: /home/ubuntu/.minikube/ca.crt
    extensions:
    - extension:
        last-update: Mon, 11 Jul 2022 13:24:34 UTC
        provider: minikube.sigs.k8s.io
        version: v1.26.0
      name: cluster_info
    server: https://192.168.49.2:8443
  name: minikube
contexts:  # 클러스터 인증 리스트 (클러스터와 유저를 묶어줌)
- context:
    cluster: minikube
    extensions:
    - extension:
        last-update: Mon, 11 Jul 2022 13:24:34 UTC
        provider: minikube.sigs.k8s.io
        version: v1.26.0
      name: context_info
    namespace: default
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users: # 인증사용자 정보
- name: minikube
  user:
    client-certificate: /home/ubuntu/.minikube/profiles/minikube/client.crt
    client-key: /home/ubuntu/.minikube/profiles/minikube/client.key
$ kubectl cluster-info
Kubernetes control plane is running at https://192.168.49.2:8443
CoreDNS is running at https://192.168.49.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

### minikube 기본 사용법

```bash
$ minikube status # 쿠버네티스 클러스터 상태 확인
$ minikube stop # 쿠버네티스 클러스터 중지
$ minikube delete # 쿠버네티스 클러스터 삭제
$ minikube pause # 쿠버네티스 클러스터 일시중지
$ minikube unpause # 쿠버네티스 클러스터 재개
$ minikube addons list # minikube 애드온 목록 확인
$ minikube addons enable [addon] # minikube 애드온 활성화
$ minikube addons disable [addon] # minikube 애드온 비활성화
$ minikube ssh # 쿠버네티스 클러스터 노드에 SSH 접속
$ minikube kubectl ... # 쿠버네티스 클러스터 버전과 대응되는 kubectl 사용
```
