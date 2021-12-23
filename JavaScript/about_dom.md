## DOM 완전 정복

### DOM 큰 그림 이해하기

- 이번 챕터에서 공부할것
  - 브라우저가 웹페이지나 웹어플리케이션을 어떻게 분석해서 정확한 위치에 표시하는지
  - 어떻게 DOM 요소를 조작할 수 있는지
  - 조금 더 깊이있게 브라우저가 렌더링 하는 순서를 공부 어떤 CSS를 써야 브라우저 렌더링 성능이 좋아지는지
- Document Object Model
  - 브라우저는 HTML 태그를 읽어서 분석한뒤 노드로 변환 (JavaScript Node)
  - 노드의 오브젝트 안에는 클래스라던지, 텍스트와 같은 입력한 모든 정보가 들어있음
  - Node는 EventTarget 오브젝트를 상속함 (모든 노드는 이벤트가 발생할 수 있음)
  - Document, Element ... 등은 노드를 상속하며, 이는 또한 이벤트가 발생할 수 있다는 것을 의미
  - **DOM**
    [Introduction to the DOM - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
  - **DOM API**
    [https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

### 우리의 조상 이벤트타겟 (EventTarget)

- **EventTarget**
  [EventTarget - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)
- 브라우저에서 HTML태그는 해석되어 엘리먼트는 Node객체로 변환됨
- Node객체는 EventTarget을 상속받아 구현되었기 때문에, EventTarget의 메소드를 사용가능
  ```jsx
  // 엘리먼트를 자바스크립트에서 변수에 할당해서 addEventListener()를 붙여서 사용가능 했던 이유
  EventTarget.addEventListener();
  EventTarget.removeEventListener();
  EventTarget.dispatchEvent();
  // 엘리먼트는 Node이고 Node는 EventTarget을 상속하기 때문
  ```
- **Node**
  [Node - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Node)
- Node에도 각종 Props와 더불어 Methods들 또한 존재하는데, 참고해봄직 함

### 웹페이지 요소 분석🔎 (콘솔툴 활용)

- Chrome의 개발툴 (cmd + shift + i) 에서 Element를 이용하면, 각각의 엘리먼트를 선택할수 있는데, $0으로 참조가 가능함

```jsx
> $0.childNodes
> NodeList(7) [text, img, text, h1#brand, text, h3, text]
```

### 알면 유용한 CSSOM (CSS Object Model)

- DOM에 대한 설명에서, 브라우저가 HTML을 해석하여 각각의 HTML태그를 DOM으로 변환한다고 했는데, CSS는 어떤식으로 처리가 될까?
- CSSOM
  [CSS Object Model (CSSOM) - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model)
- HTML태그 자체는 DOM으로 정의되고, 태그 내의 인라인 CSS나 파일에서 정의한 CSS등은 별도 해석되어 DOM의 구조로, 각각의 엘리먼트에 정의된 CSSOM이라는 형태로 만들어짐
- CSSOM에서는 유저가 정의한 CSS외에도 브라우저의 기본CSS등이 포함되어 있음 = computed styles
- CSS는 cascading룰에 의거, 부모에 정의된 룰이 자식으로 전파됨
- **Render Tree**
  - DOM (HTML의 해석에 의해 엘리먼트의 트리로 만들어짐)
  - CSSOM (각각의 요소에 어떤 CSS가 최종적으로 적용되어야 하는가에 대한 룰셋)
  - 상기 2개를 합쳐서 Render Tree의 형태로 적용됨
  - 렌더 트리는 사용자에게 보여지는 파트를 표기하는것이기 때문에, DOM에서도 보이는 부분만 포함됨
    - opacity가 0이거나, visibility가 hidden인 경우에는 Render Tree에는 포함이 됨
    - display가 none인 경우에는 Render Tree에 포함되지 않음
- 정리
  - 브라우저는 DOM → CSSOM → Render Tree를 작성하여 사용자에게 보여줌

### 성능 보장 렌더링 순서 ⚡️

- 웹페이지나 웹어플리케이션의 렌더링 과정에 대해서 공부
- 브라우저에서 URL을 입력하게 되면 다음의 순서로 진행
  1. requests/response (HTML)
  2. loading
  3. scripting (HTML → DOM)
  4. rendering (Render Tree 작성)
  5. layout
  6. painting
- Construction
  1. DOM
  2. CSSOM
  3. Render Tree
  - 상기 이 과정을 빠르게 하기 위한 방법
  - DOM의 요소가 작으면 작을수록, CSS의 크기가 작으면 작을수록
  - 불필요한 태그 남용, 쓸데없는 wraping 클래스의 남용 금지
- Operation
  1. layout
     - Render Tree (DOM + CSSOM) 을 이용해서, 각각의 요소가 어느정도 위치일지 구상
     - x, y, width, height 등을 계산
  2. paint
     - 계산된 내용을 바로 그리는것이 아님
     - 어떻게 배치되어 있는지에 따라 이미지를 파트별로 잘게 나누어서 준비 (비트맵)
       - z-index 값과 같이 같은 레이어상에 있다면 레이어별로 준비한다는 뜻
       - 브라우저의 성능개선을 위해서 일부러 레이어 기능을 이용함 (특정 레이어상의 요소가 변경되면 해당 레이어만 리렌더링하기 위해서)
       - CSS에서 will-change 속성이 있다면, 브라우저가 새로운 레이어에 추가함 (남용금지)
  3. composition
     - 브라우저상에 표기하는 순서를 정해서 표기
  - 오퍼레이션 과정을 빠르게 하기 위한 방법
  - paint가 자주 일어나지 않도록 하자
  - translate를 이용하면 composition만 일어나기 때문에 paint는 발생하지 않음
  - layout이 바뀌면 최악 → 하나의 요소의 이동에 의해 주변요소가 영향을 받는 변경을 되도록이면 자제

### 모르면 후회하는 레이어 데모

- CSS상에서 will-change를 입력해서 브라우저에게 변경을 알려주면 브라우저는 해당 파트를 별도 레이어로 만들어냄
- 레이어는 브라우저 개발툴의 요소 → 레이어에서 확인 가능

### 즐겨찾기하면 좋은 사이트 모음 👨‍💻

[CSS Triggers](http://csstriggers.com/)

- CSS 속성값이 좋은지 안좋은지 확인
- 어떤 속성값이 layout, paint, composition이 발생하는지 확인할 수 있는 사이트
- Blink (크롬)
- Gecko (Firefox)
- Webkit (Safari)
- EdgeHTML (구Edge)
- 좋은 예 (composition만 발생) 😘
  - opacity
  - transform
- 나쁜 예 (layout 유발자) 😭
  - width
  - height

### DOM 조작하기

- querySelector()
  [Document.querySelector() - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)

```jsx
const image = document.querySelector('img');
console.log(image);

// querySelector()는 첫번째 찾아진 Element를 리턴한다
// 찾지 못하면 null을 리턴한다
// querySelector()는 CSS에서 쓸수있는 선택자를 이용해서 DOM을 받아올 수 있다
// Edge에서는 12버전, 인터넷 익스플로러에서는 8부터 지원이 된다
// 그 전에는 getElementById()를 이용했다
```

- 기존 getElementById(), getElementsByClassName()과의 차이점은 같은 API를 이용하면서 쿼리셀렉터를 이용해서 필터링을 할 수 있다는 점
- querySelectorAll()
  [https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)
  - querySelectorAll()은 모든 엘리먼트를 받아오며, 리스트 형태로 반환
- DOM의 작성
  - createElemement()
  ```jsx
  const section = document.querySelector('section');
  const h2 = document.createElement('h2');
  h2.setAttribute('class', 'title'); // <h2 class="title"></h2>
  h2.textContent = 'This is a title'; // <h2 class="title">This is a title</h2>
  section.appendChild(h2);
  // section.insertBefore(newNode, referenceNode)
  const h3 = document.querySelector('h3');
  section.insertBefore(h2, h3); // h3직전에 h2가 들어감
  ```

### innerHTML vs createElement 뭘쓰지?

- innerHTML
  - 전체 HTML코드를 받아오거나 수정이 가능
- element조작
  - 레퍼런스를 포함하고 있기 때문에, 여러가지 API를 이용
  - 업데이트 이후에도 참조값이 있기 때문에 삭제하거나 하는 등 다이나믹한 처리가 가능
