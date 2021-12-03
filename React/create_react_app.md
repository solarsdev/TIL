## Create React App

- Create React App을 이용해서 React 어플리케이션을 시작하는 방법
- 공식적으로 제공하는 방식이며 가장 편하고 안전하게 React 어플리케이션 개발환경을 구성할 수 있음

### Introduction

- Create React App을 이용하기 위해서는 다음과 같은 요구사항이 적용됨
  - NodeJS의 인스톨
    [Node.js](https://nodejs.org/ja/)
  - NodeJS를 설치한 뒤에는 npx를 이용할수 있는지 확인
  ```bash
  npx create-react-app my-app
  cd my-app
  npm start
  ```
- Visual Studio Code를 이용해서 프로젝트 폴더를 연 뒤에 npm start를 이용해서 코드를 실행
- React에서 기본으로 제공하는 코드에서 필요없다면 지워줘야 할 파일들
  ```jsx
  App.css;
  App.test.js;
  index.css;
  logo.svg;
  reportWebVitals.js;
  setupTests.js;
  ```

### Tour of Ceate React App

- NodeJS 환경에서 작업을 진행하면서 얻는 이점
  - VisualStudio Code의 자동완성 기능을 사용가능
  - 각종 라이브러리들을 간편하게 인스톨 가능 (npm install)
  - 파일을 분리하여 관리 (import export)
- CSS의 모듈화

  - filename.module.css를 통한 CSS 모듈링

  ```css
  .btn {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell,
      'Open Sans', 'Helvetica Neue', sans-serif;
    font-size: 18px;
    background-color: tomato;
    color: white;
  }
  ```

  ```jsx
  import PropTypes from 'prop-types';
  import styles from './Button.module.css';

  const Button = ({ text }) => {
    return <button className={styles.btn}>{text}</button>;
  };

  Button.propTypes = {
    text: PropTypes.string.isRequired,
  };

  export default Button;
  ```

  ```html
  <button class="Button_btn__1uuCO">Continue</button>
  <!-- class이름이 랜덤하게 결정되면서 다른 컴포넌트들의 클래스명과 중복되지 않도록 해준다 👍 -->
  ```
