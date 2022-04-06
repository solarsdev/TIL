# 자바스크립트란?

- 자바스크립트는 Stack Overflow에서 질문수 랭킹 1위에 해당할 만큼 압도적인 인지도를 자랑
- JavaScript를 배워두면 프론트엔드, 백엔드 가릴것 없이 개발이 가능
- 다양한 웹 어플리케이션용 언어들에 연관되어 있어 다른 언어를 배우기 위한 첫 시작으로 알맞음
  - HTML/CSS
  - Node.js
  - TypeScript
  - 기타 JavaScript 관련 프레임워크들
- 자바스크립트를 배워서 사용할수 있는 영역

  1. 브라우저

     HTML을 이용해서 골격을 만든 후 CSS를 이용해서 꾸미고 나면, JavaScript를 이용해서 동적인 데이터 관리가 가능해짐

  2. 컴퓨터

     `Node.js`를 이용해서 JavaScript 언어로 동작하는 백엔드 프로그램을 작성할 수 있음

# 자바스크립트 엔진에 대해서

- 어떻게 자바스크립트 언어가 동작하는지에 대한 개념원리
- 브라우저에서 자바스크립트가 동작하기 위해 자바스크립트 엔진이 필요
- 자바스크립트가 동작하는 기간을 런타임이라고 하고, 이 시간에 자바스크립트 엔진은 자바스크립트 소스코드를 한줄씩 읽어가며 실행하게 됨
- 자바스크립트 소스코드를 읽어서 엔진에 번역해서 전달해주는것이 인터프리터
- 다른 프로그래밍 언어에서는 컴파일러가 필요
- 컴파일러와 인터프리터의 차이점
  1. 컴파일러
     - 개발자가 작성한 코드를 컴퓨터가 실행할수 있는 실행파일로 변환
     - 컴파일링과정을 거쳐서 실행파일을 이용해서 컴퓨터에서 실행
     - 실행을 위해서는 모든 소스코드를 컴파일해야 함
     - 컴파일 과정에 시간이 소요된다는 단점이 있지만, 미리 실행파일을 만들어두기 때문에 실행시간에는 좀 더 빠르게 수행할 수 있다는 장점이 있음
     - 컴파일을 하게 되면 모든 변수가 정적으로 정의됨
  2. 인터프리터
     - 소스코드를 실행시간에 실시간으로 번역해가면서 실행하기 때문에 초기 실행속도는 빠르지만 진행속도는 비교적 느리다는 단점이 있음
     - 최근의 인터프리터는 최적화 및 개선이 이루어져 있기 때문에 실행속도에서 그렇게 손해를 보는 편은 아님

# ECMAScript란?

- 각각의 브라우저도 저마다의 엔진을 가지고 있는데 아래와 같음
  1. IE
     - Chakra
  2. Chrome
     - V8
  3. Safari
     - JavaScript Core
  4. Firefox
     - SpiderMonkey
  5. Microsoft Edge
     - V8
  6. Node.js
     - V8
- 처음 크롬에서 V8엔진을 개발했을때 Just In Time Compilation이라는 컴파일과 인터프리터의 장점을 결합해서 만들어낸 신기술을 적용했는데, 성능이 워낙 좋다보니 여러곳에서 이를 채용하기 시작함
- 브라우저 엔진이 자바스크립트를 이해하고 실행하기 위해서는 공통된 규격이 필요한데 이를 ECMAScript라고 부름
- ECMAScript는 자바스크립트 문법의 규격사항, 표준사항을 망라한 것으로서, 각각의 브라우저 엔진들이 이를 토대로 각자의 구현방법을 정의한것임
- ECMAScript는 자바스크립트가 처음 발표된 1995년부터 그 명맥이 지속되어 왔는데, 역사가 오래된 만큼 각 버전별로 특정 기능이 지원되거나 지원되지 않거나 하는 특징이 있음
  [ECMAScript - Wikipedia](https://en.wikipedia.org/wiki/ECMAScript)
  - 주요한 변경점으로서 ES5, ES6등이 있는데 ES5에서는 regex, JSON에 대한 대응등이 추가되었고 ES6(현시점 자바스크립트의 기본문법이라고 정의함)에서는 class나 import등에 대한 대응이 추가되었음
- 자바스크립트에서는 이렇게 다양한 버전에 따라 신기능을 추가하고 있지만 babel을 이용하면 최신문법을 구버전의 브라우저가 해석할수 있는 낮은 버전의 JavaScript로 번역해주기 때문에 개발시에는 최신 문법을 사용하고 퍼블리싱을 할때 번역된 버전을 배포하는 식으로 관리를 해주면 됨
  - 각각의 브라우저 버전에 따라 어떤 ECMAScript를 지원하는지 확인하기
    [ECMAScript 5 compatibility table](https://kangax.github.io/compat-table/es5/)

# Definition of JavaScript

[JavaScript - Wikipedia](https://en.wikipedia.org/wiki/JavaScript)

- JavaScript is `lightweight`, `interpreted`, or `just-in-time compiled` programming language with `first-class functions`. While it is most well-known as the scripting language for `Web pages`, many non-browser environments also use it, such as `Node.js`, `Apache CouchDB and Adobe Acrobat`.
- JavaScript is a `prototype-based`, `multi-paradigm`, `single-threaded`, `dynamic language`, supporting `object-oriented, imperative, and declarative` (e.g. functional programming) styles.
