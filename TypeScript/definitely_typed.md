### DefinitelyTyped

- CRA에 TypeScript를 추가하는 방법으로는 앱을 생성하는 시점에서 타입스크립트의 형태로 만들기
- 기존 앱상에서 타입스크립트를 수동으로 설치해서 변환하는 방법

[Adding TypeScript | Create React App](https://create-react-app.dev/docs/adding-typescript/)

```bash
# CRA로 앱 만들때 템플릿을 타입스크립트의 형태로 만들기
npx create-react-app my-app --template typescript

# 기존 프로젝트에 타입스크립트 패키지 설치하기
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

- 기존 프로젝트를 변경할 경우 타입스크립트를 인스톨한 뒤에 파일명 변경
  - App.js → App.tsx
  - index.js → index.tsx
- 변경 후에 JavaScript 라이브러리등에서 에러가 발생할 수 있음
  - 예를 들어, styled-components 등에서 에러가 발생하면 해결방법으로는 타입스크립트에 정의문을 추가해줄 필요가 있음
    [@types/styled-components](https://www.npmjs.com/package/@types/styled-components)
    ```bash
    npm install -D @types/styled-components
    ```
  - 스타일 컴포넌트와 같이 JavaScript 전용 라이브러리의 경우에는 오픈소스 형태로 개발자들이 소스를 분석해서 타입스크립트의 정의문을 제공하는데, 이것이 @types 라이브러리에 있는 styled-components임
- 개발자들이 모여서 만든 타입정의 커뮤니티를 DefinitelyTyped 라고 부르고 npm의 @types를 찾아보자
