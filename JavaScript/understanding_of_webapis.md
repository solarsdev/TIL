## Web APIs에 대한 이해

### Web API란 무엇인가?

- Application Programming Interface의 약어
- 각종 플랫폼에서 제공하는 데이터를 이용하기 위한 규약
  - 예를 들어 전원 케이블에 소켓을 꼽기만 하면 내부 설계와는 관계없이 바로 이용할수 있음
  - Windows에서 제공하는 API를 이용해서 윈도우 어플리케이션을 제작 가능
  - 유튜브에서 백엔드 서비스 데이터를 API로 제공하는데, 그것을 바로 받아올 수 있음
  - 직접 작성한 클래스의 UserStorage에서 Login, Logout 기능을 제공한다면, 그것 또한 API로 제공하는것이라고 말할 수 있음 (사용자는 내부 기능이 어떻게 구현되어 있는지는 상관없이 바로 원하는 기능을 이용할수 있음)
- 브라우저마다 공통적으로 제공하기로 한 API가 많이 있음
  - DOM APIs (Document Object Model) 웹페이지의 요소를 생성, 삭제, 스타일을 제어
  - Network APIs
  - Graphics APIs Canvas와 같은 것들을 이용할 수 있음
  - Audio/Video APIs 멀티미디어 관련 기능
  - Device APIs 사용자가 온라인인지 오프라인인지, 디바이스의 상태 정보
  - File APIs 사용자의 파일을 읽고 쓰기
  - Storage APIs 브라우저 상에서 임시 정보들을 관리할수 있도록 도와줌
- 어떤 API가 존재하고 어떻게 활용하는지는 어떻게 알수 있을까?
  - MDN Document
    [Introduction to web APIs - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Introduction)
    [Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API)
    [Here's What JavaScript CAN'T Do](https://www.thoughtco.com/what-javascript-cannot-do-2037666)
    - 쓰레기통 아이콘은 사용하면 안됨 (Deprecated or will be)
    - 경고 API는 최신 내용이 추가된 것들이라 사용할때 주의가 필요
  - 예를 들어 Geolocation API
    - HTTPS일 때만 사용가능하다는 경고 메시지가 출력되어 있음
    - Web APIs에서는 보안을 중요시 하기 때문에, 사용자의 권한 요청이나 HTTPS를 요구할 수 있음
    - HTTP (Hypertext Transfer Protocol)
      - request와 response로 이루어져 있는 통신규약
    - HTTPS (Hypertext Transfer Protocol Secure)
      - request와 response로 이루어져 있지만, 암호화를 통해 보안을 강화한 버전
  - External APIs
    - Twitter, Trello, Youtube, Pinterest, Weather APIs
    - 외부 서비스들도 각자 APIs를 제공하기 때문에 백엔드 데이터에 쉽게 접근할수 있는 방법이 있음

### Browser 구조 분석

- 브라우저에서 웹페이지를 열면 Window라는 오브젝트가 존재
  - Window Object
    - DOM Document Object Model (HTML에서 작성한 요소들이 표기되는 부분)
    - BOM Browser Object Model (Navigator, Location, Fetch, Storage ...)
    - JavaScript (Array, Map, Date ...)
- Chrome의 개발자툴을 이용해서 직접 window 객체를 출력해보면 다양한 함수와 객체들이 있는것을 확인할 수 있음
- window객체는 global object이므로 아무런 오브젝트를 지정하지 않으면 window내에 있는 함수 및 객체를 이용하게 됨
  ```jsx
  // 아래의 커맨드는 동일함
  window.innerWidth; // == innerWidth
  innerWidth; // == window.innerWidth
  ```
- 참고 자료
  - Document
    [Document - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document)
  - Window
    [Window - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window)
  - Viewport
    [Layout viewport - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/layout_viewport)
  - Navigator
    [Navigator - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Navigator)

### 실습1. 윈도우 사이즈 표기

- 다음 4가지를 출력하는 어플리케이션을 만들어보자
  - window.screen
    - 모니터의 해상도 (브라우저 밖에 있는 것들을 다 합한 사이즈)
  - window.outer
    - URL, 탭 등 전체적인 브라우저의 사이즈
  - window.inner
    - 웹페이지, 스크롤바 포함해서 표시되는 전체 사이즈
  - documentElement.clientWidth
    - 문서의 사이즈, 스크롤바가 있다면 제외한 영역
- 실습 포인트
  - 브라우저의 크기가 변경되면 실시간으로 업데이트 되도록 하기
  - window-size.html

```html
<p style="background-color: lightpink">
  <span>window.screen: </span><span id="screen"></span><br />
  <span>window.outer: </span><span id="outer"></span><br />
  <span>window.inner: </span><span id="inner"></span><br />
  <span>document.Element.clientWidth: </span><span id="documentClientWidth"></span>
</p>
```

```jsx
const screen = document.getElementById('screen');
const outer = document.getElementById('outer');
const inner = document.getElementById('inner');
const documentClientWidth = document.getElementById('documentClientWidth');

const updateSize = () => {
  screen.innerText = `${window.screen.width}, ${window.screen.height}`;
  outer.innerText = `${window.outerWidth}, ${window.outerHeight}`;
  inner.innerText = `${window.innerWidth}, ${window.innerHeight}`;
  documentClientWidth.innerText = `${document.documentElement.clientWidth}, ${document.documentElement.clientHeight}`;
};

updateSize();
window.onresize = updateSize;
```

### 브라우저 좌표

- 브라우저의 X축과 Y축에 대한 정리
  - X는 수평축
  - Y는 수직축
  - 좌표는 document의 왼쪽, 최상단에서 (0, 0)으로 시작함
  - Element.getBoundingClientRect()
    - Element 오브젝트의 함수
    - Element라는 것은 DOM에 들어있는 모든 요소들 (이미지, 텍스트등 다양한 태그들)
    - 요소의 위치나 크기등에 대한 정보를 얻어올 수 있음
    - top, left, width, height 등
    - left == x, top == y
    - bottom, right은 태그의 오른쪽 최하단이 됨
    - CSS에서의 right, bottom과는 의미가 조금 다른것이 CSS에서는 브라우저의 외곽에서의 거리를 의미한다면, getBoundingClientRect()에서의 right, bottom은 브라우저의 최상단, 왼쪽에서의 거리를 의미함
- Client x, y vs Page x, y
  - 마우스 클릭시에 발생하는 이벤트에서 구할 수 있는 값
  - Client x, y의 의미는 페이지에 상관없이 브라우저 윈도우 창에서 x, y가 어느정도 떨어져 있는지가 전달됨
  - Page x, y는 페이지를 구성하는 최상단과 왼쪽에서 어느정도 떨어져 있는지가 전달됨 (스크롤링에 대한 이동분도 포함되어 계산되는 구조)

### 실습2. 좌표 실습

- 요구사항
  - div를 복수 만들고, 특정 div만 다른 색상으로 지정할것
  - 다른 색상으로 지정된 div를 클릭하면 client x,y와 page x,y를 출력
  - window-coordinate.html

```html
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box target"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
```

```css
body {
  background-color: black;
}
.box {
  background-color: bisque;
  width: 250px;
  height: 250px;
  margin: 10px;
}
.target {
  background-color: pink;
}
```

```jsx
const target = document.querySelector('.target');
target.addEventListener('click', (event) => {
  console.log(target.getBoundingClientRect());
  console.log(`client: ${event.clientX}, ${event.clientY}`);
  console.log(`page: ${event.pageX}, ${event.pageY}`);
});
```

### 실습3. 윈도우 스크롤링 APIs

- 버튼 3개를 이용
  - 100px y좌표
  - 100px x좌표
  - 항상 target으로 이동

```css
body {
  background-color: black;
}
.sticky {
  position: sticky;
  top: 10px;
}
.box {
  background-color: bisque;
  width: 250px;
  height: 250px;
  margin: 10px;
}
.target {
  background-color: pink;
}
```

```html
<div class="sticky">
  <button id="btn100y">Scroll by 100px(y)</button>
  <button id="btn100x">Scroll by 100px(x)</button>
  <button id="btnTarget">Scroll into target</button>
</div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box target"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
```

```jsx
const target = document.querySelector('.target');
target.addEventListener('click', (event) => {
  console.log(target.getBoundingClientRect());
  console.log(`client: ${event.clientX}, ${event.clientY}`);
  console.log(`page: ${event.pageX}, ${event.pageY}`);
});
const btn100y = document.getElementById('btn100y');
btn100y.addEventListener('click', (event) => {
  window.scroll(event.pageX, event.pageY + 100);
});
const btn100x = document.getElementById('btn100x');
btn100x.addEventListener('click', (event) => {
  window.scroll(event.pageX + 100, event.pageY);
});
const btnTarget = document.getElementById('btnTarget');
btnTarget.addEventListener('click', (event) => {
  const targetDOMRect = target.getBoundingClientRect();
  console.log(targetDOMRect);
  window.scrollBy(targetDOMRect.x, targetDOMRect.y);
});
```

### 자바스크립트에서 'use strict';를 사용하는 이유

- 타입스크립트를 사용할때는 선언할 필요가 없지만 순수 vanilla 자바스크립트를 사용할때 활용하면 좋음
- 자바스크립트의 유연함이 장점일수 있지만, 개발과정에서는 단점으로 작용할 수 있음
- 기본 자바스크립트에서는 선언되지 않은 변수를 사용한다거나, 기존 변수의 프로토타입을 변경할 수 있음
- ECMAScript5에서 추가된 기능으로 'use strict'; 를 맨 윗줄에 추가하면, 변수 관련 선언 에러를 명시적으로 표시해주기 때문에 활용해야 함

```jsx
'use strict';
console.log('Hello, World!');

myVariable = 6; // ERROR! 명시되지 않은 변수
```

### Window load의 비밀

- 브라우저에서 HTML코드를 해석하다가 리소스를 만나면 필요한 파일들을 다운로드 받게 됨
  [자바스크립트 2. 콘솔에 출력, script async 와 defer의 차이점 및 앞으로 자바스크립트 공부 방향 | 프론트엔드 개발자 입문편 (JavaScript ES5+)](https://www.youtube.com/watch?v=tJieVCgGzhs)
- **head내에 script를 일반적으로 작성하는 방법**
  ```jsx
  <head>
    <script src='main.js'></script>
  </head>
  ```
  1. HTML을 해석하던 도중 그 과정이 중단되고 스크립트를 fetch하여 실행하게 됨
  - 사용자가 웹사이트를 로딩하는데 오래 걸리게 됨
  - 스크립트의 fetch가 완료되어 실행될때 DOM요소가 존재하지 않을 가능성이 큼
- **body 제일 끝에 script를 작성하는 방식**
  ```jsx
  <body>
    <script src='main.js'></script>
  </body>
  ```
  1. HTML을 해석완료 하고나서 JavaScript 파일을 fetch하고 실행
  - 웹페이지의 작동방식이 스크립트에 의존적이라고 하면, 결국 자바스크립트를 완전히 로딩해서 실행하기 전까지는 제대로 표시하는것이라고 보기는 힘들 것임
- **head에 작성하되 async를 붙여서 사용하는 방법**
  ```jsx
  <head>
    <script async src='main.js'></script>
  </head>
  ```
  1. 해당 스크립트 구문을 만났을때는 HTML의 해석과 병렬로 fetch 과정을 수행
  2. fetch가 완료된 순서대로 (정의순서 아님) 스크립트를 실행
  - 스크립트의 fetch가 완료되어 실행될때 DOM요소와의 연계기능이 있다면 아직 해석이 완료되지 않아서 존재하지 않을 가능성 내포
  - HTML 해석 도중에 스크립트 실행을 해야 하기 때문에, 웹페이지의 로딩을 지연시킴
- **head에 작성하고 defer를 붙여서 사용하는 방법**
  ```jsx
  <head>
    <script defer src='main.js'></script>
  </head>
  ```
  1. 해당 스크립트 구문을 만났을때는 HTML의 해석과 병렬로 fetch 과정을 수행
  2. HTML의 해석이 종료되어 웹페이지가 ready 상태가 되었을때 정의한 순서대로 스크립트를 실행
  - body 끝에 작성하는 것과 비슷하게 자바스크립트를 완전히 로딩해서 실행하기 전까지는 제대로 표시할수 없겠지만, HTML해석과정에서 fetch를 병렬로 처리하기 때문에 속도면에서 이득이 있음
  - 결국 현존 여러가지 방법중에는 더이상 깔게 없는 좋은 방식 👍
- 이벤트 리스너 로딩 관련
  ```jsx
  // only document
  window.addEventListener('DOMContentLoaded', () => {
    console.log('DOMContentLoaded');
  });

  // after resource (css, images)
  window.addEventListener('load', () => {
    console.log('loaded');
  });

  window.addEventListener('beforeunload', () => {
    console.log('before unload');
  });

  window.addEventListener('unload', () => {
    console.log('unload');
  });
  ```
  1. defer가 가장 먼저 실행됨 (HTML파싱 완료 직후에 실행됨)
  2. DOMContentLoaded (HTML파싱 완료 직후에 실행됨)
  3. load (css나 images와 같이 소스에 추가된 리소스들이 전부 다운로드 되어 로딩되고 나서 수행됨)
  4. beforeunload (브라우저가 종료되기 직전에 수행됨)
