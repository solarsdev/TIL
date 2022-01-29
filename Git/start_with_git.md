## Git의 최초 설정

### 사용자 설정

```bash
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

### 코어 에디터 설정

```bash
git config --global core.editor "code --wait"
git config --global autocrlf input | [windows true]
```

## Git을 시작하는 방법 (init)

- 깃은 특정 폴더에 .git 디렉토리를 작성하는것부터 시작하게 됨
- .git 디렉토리는 깃의 컨셉을 이해하는데도 중요한 디렉토리가 되는데, 디렉토리 내 모든 변경사항을 기록하는 장소임
- Git을 시작하기 위해서 다음 두가지 케이스가 존재함
  1. 아직 버전관리를 시작하지 않은 로컬 디렉토리에서 Git을 시작
  2. 다른 어딘가의 Git 저장소를 clone을 통해서 받아오기

### 기존 디렉토리를 Git 저장소로 만들기

```bash
# git init을 수행하면 .git 폴더를 작성하고 작성된 시점부터 변경이력을 추적하게 됨
git init
```

- 상기 명령을 통해 `.git` 디렉토리를 만듦
- Git이 파일을 관리하게 하려면 저장소에 파일을 추가하고 커밋을 수행해야 함
- `git add` 명령으로 파일을 추가하고 `git commit` 명령으로 커밋함

### 기존 Git 저장소를 Clone 하기

- 다른 프로젝트에 참가하거나 Git 저장소를 복사하고 싶을때 `git clone` 명령을 사용
- checkout이 아닌 clone으로 명령어가 정해진 이유는 원격지 서버의 거의 모든 정보를 그대로 복사하기 때문인데, 이는 CVCS와의 차이점으로 각각의 로컬 저장소에서 분산해서 관리하는 DVCS의 특징이기도 함
