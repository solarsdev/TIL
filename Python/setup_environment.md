# Setup Environment

## The package installer for python

- pipenv

### pipenv와 같은 패키지 인스톨러를 사용해야 하는 이유

- nodejs와는 다르게 파이썬은 공식적인 npm이 없음
- python에서는 pip를 제공하는데 npm과는 다르게 pip는 다운받는 모든 라이브러리를 전역적으로 설치함 (npm install -g something)
- npm은 기본적으로 프로젝트별 패키지 관리를 지원하지만 pip은 지원하지 않는다는 이야기
- 위와 같은 이유에서 pipenv가 필요하다

### pipenv

- pipenv는 파이썬을 위한 `npm + package.json` 과 같다고 생각하면 됨
- package.json 대신 pipenv는 Pipfile을 생성함 (패키지리스트)

### 개발환경 폴더 작성 및 env 생성

```bash
cd projects/project_name
pipenv --three
```

- 개발환경 폴더에는 `Pipfile`이 작성됨 (npm의 package.json)
- pipenv를 작성하는것은 가상 환경을 빌드한다고 생각하면 됨
- 가상환경은 작성된 이후에 진입해야 할 필요성이 있음

```bash
pipenv shell #생성된 environment로 진입
Launching subshell in virtual environment...
. /virtual-environment-YhuHpRQS/bin/activate
pipenv install Django
```

## Testing the bubble

```bash
# not in bubble
django-admin
zsh: command not found: django-admin
pipenv shell #생성된 environment로 진입
Launching subshell in virtual environment...

# in bubble
django-admin
[응답 생략]
```
