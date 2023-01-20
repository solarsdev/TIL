# Node Package Manager

## NPM에 대해서

[CLI Commands | npm Docs](https://docs.npmjs.com/cli/v9/commands)

- 노드 패키지 매니저인 npm은 전세계에서 가장 큰 라이브러리 플랫폼 (백만개가 넘는 라이브러리 존재)
- 다양한 대신 라이브러리의 품질이 천차만별이기 때문에 제대로 선정하는 것이 중요
- package.json 메타데이터 파일이 라이브러리를 정의하여 언제든지 받아올 수 있음
  - 따라서 라이브러리 바이너리를 공유할 필요 없이 메타데이터만 공유해도 됨

### npx

- npm이 바이너리를 다운로드 받아오는 것이라고 하면, npx는 일회성 바이너리를 바로 실행하기 위함

### yarn

- npm의 보안 및 속도를 개선한 새로운 패키지 관리자 (by facebook)

## 라이센스에 대해서

[ISC License](https://www.olis.or.kr/license/Detailselect.do?lId=1074&mapCode=010074&lType=osi)

[Software Package Data Exchange (SPDX)](https://spdx.org/licenses/ISC.html)

- 라이센스에 대한 정보를 찾아보고, 적절한 라이센스를 규정하는 것은 추후 법적인 문제를 겪지 않기 위해 중요

## 버전 정보

- 프로젝트를 작성할 때 버전을 기재할 때 사용하는 방식 (시맨틱 )
  - 1.0.0 (major.minor.patch)
  - patch: 버그 수정
  - minor: 기능 추가
  - major: 전혀 다른 기능, 기존 시스템에 영향이 생길 수 있는 변화 등
- npm에서 활용할 수 있는 버전 표기에 대해서 설명
  [About semantic versioning | npm Docs](https://docs.npmjs.com/about-semantic-versioning)
- 시맨틱 버전 테스터
  [npm semantic version calculator](https://semver.npmjs.com/)

### npm install

```bash
Install a package

Usage:
npm install [<package-spec> ...]

Options:
[-S|--save|--no-save|--save-prod|--save-dev|--save-optional|--save-peer|--save-bundle]
[-E|--save-exact] [-g|--global] [--global-style] [--legacy-bundling]
[--omit <dev|optional|peer> [--omit <dev|optional|peer> ...]]
[--strict-peer-deps] [--no-package-lock] [--foreground-scripts]
[--ignore-scripts] [--no-audit] [--no-bin-links] [--no-fund] [--dry-run]
[-w|--workspace <workspace-name> [-w|--workspace <workspace-name> ...]]
[-ws|--workspaces] [--include-workspace-root] [--install-links]

aliases: add, i, in, ins, inst, insta, instal, isnt, isnta, isntal, isntall
```

- 글로벌로 설치하기
  - npm에 -g 옵션을 이용하면 글로벌로 설치할 수 있음
  - 글로벌로 설치할 때 sudo를 이용한 권한 엘리베이션은 보안문제가 발생할 여지가 있으므로 다음의 명령어로 대신 사용할 것
    ```bash
    sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
    ```
- 설치할 때 package-lock.json에 대해서
  - 패키지 lock은 정확하게 어떤 버전을 사용했는지 기재하기 위함
- 업데이트 하기
  ```bash
  npm outdated
  npm update
  ```
- 개발 모드로 설치하기
  - npm i [package] —save-dev
  - 프로덕션에 배포할때는 필요없는 개발환경에서만 필요한 패키지를 설치하기 위함
