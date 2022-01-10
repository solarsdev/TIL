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

### 실습7. 쇼핑 목록앱 만들기

- 요구사항
  - ToDo리스트처럼 장바구니에 간단하게 원하는 상품을 기재하고 리스트에 추가하는 기능
  - 리스트에서 상품을 삭제하는 기능
  - 브라우저의 스토리지 기능을 이용해서 세션이 끊어져도 기억하는 기능
  - 디자인

## 이벤트

### 이벤트 정확하게 이해하기 (+이벤트의 종류들)

- 브라우저에서 발생할수 있는 이벤트는 다양함
  - 예를들어, 어떤곳을 클릭했을때도 이벤트가 발생하지만, 그 외에도 아래와 같은 이벤트도 발생함
  - 키보드
  - 리사이징
  - 윈도우 닫기
  - 페이지 로딩
  - 양식 제출
  - 비디오가 재생됨
  - 에러
- 다양한 이벤트들을 직접 살펴보기 (MDN사이트에서 이벤트 레퍼런스 확인)
  [Event reference | MDN](https://developer.mozilla.org/en-US/docs/Web/Events)
- 이러한 다양한 이벤트들을 모두 처리할 필요는 없고, 필요한 이벤트를 원하는 엘리먼트에 걸어서 사용할 수 있음
- 특정 엘리먼트에 이벤트를 등록하면 됨
- 엘리먼트에서 이벤트가 발생하면 이벤트를 오브젝트 형태로 만들어서 등록한 콜백함수에 전달해줌
- EventTarget에는 addEventListener, removeEventListener, dispatchEvent가 존재하는데, 이 중 dispatchEvnet는 인공적으로 이벤트를 발생시키는것을 말함

### 실습8. 필수로 알아야 하는 버블링 & 캡쳐링

- 실습코드를 받은 뒤, Click Me를 누르게 되면 버튼 요소 뿐만 아니라 outer와 middle에서도 이벤트가 발생하는 것을 확인할 수 있음
- 왜 outer와 middle에서 이벤트가 발생했을까?
  - 브라우저에서 이벤트를 처리하는 캡쳐링과 버블링 때문
  - 캡쳐링은 특정 요소에서 이벤트를 발생시킬때, 부모로부터 차근차근 해당 객체까지 식별해 나가는 단계를 말함
  - 버블링은 특정 요소에서 특정 이벤트를 발생시키면, 부모에게도 해당 이벤트가 발생되었음을 알려주는것을 말함
    <aside>
    💡 캡쳐링 단계에서는 자바스크립트에서 어떤 작업을 수행해야 하는 경우가 거의 없기 때문에, 일단 버블링에 집중해서 공부 해보자
    
    </aside>

- event의 stopPropagation()을 이용하면 부모에게 이벤트를 전달하는것을 막을 수 있음 (버블링 금지)
- event의 stopImmediatePropagation()을 이용하면 해당 콜백함수만 실행한뒤 다른 리스너의 실행까지 금지
  - 단, 이벤트의 등록 순서에 따라 실행 순서가 결정되기 때문에, 버튼의 등록된 이벤트의 순서상 중간에 실행할 경우에는 그 전까지의 이벤트는 발생하게 됨
- stopPropagation, stopImmediatePropagtion은 되도록이면 사용하지 않는것이 좋음
  - stopPropagation을 사용한다는 것은, 다른 어떤 이벤트들이 있더라도 무시하고 실행하지 않는것을 말하기 때문에, 다른 어떤 처리가 있는지 무시한채 중지하게 됨
  - 프로젝트의 규모가 커질 경우, 해당 처리때문에 오랜기간 디버깅을 해야 할 수도 있음
  - 그럼 stopPropagation을 대체하려면 어떤것을 해야 하면 될까?
  ```tsx
  // 처리해야 하는 이벤트의 타겟과 currentTarget이 일치하지 않으면 종료하는 문구를 추가
  if (event.target !== event.currentTarget) {
    return;
  }
  ```

### 브라우저의 기본 기능을 취소하기 🚨

- 일반적으로 사용하는 브라우저의 기본 동작을 취소하는 방법
  - event.preventDefault()
  - 패시브 이벤트에서는 preventDefault()를 사용할 수 없음
  - 액티브 이벤트는 무엇이고 패시브 이벤트는 무엇이길래 패시브 이벤트에서는 preventDefault()를 사용할수 없다는 걸까
    - 브라우저의 특정 기능들은 액티브, 패시브가 나누어져 있는데, 그 기준은 다음과 같다
      - 액티브 이벤트: 이벤트에 로직을 넣을 경우, 해당 로직이 전부 실행될때까지 기다렸다가 수행하는 것
      - 패시브 이벤트: 예를 들면 스크롤, 빠르게 무언가 해야 하는 경우에는 브라우저에서 담당한 내용을 무조건 수행함
  - 그렇다면 패시브 이벤트는 절대 취소할수 없는걸까?
    - 할수 있음
    - addEventListener()를 이용해서 이벤트를 등록할때 passive를 false로 주게 되면, 강제적으로 해당 리스너를 액티브하게 만들 수 있음
    - 다만, passive가 기본적으로 true로 설정된 요소에는 강제적으로 액티브로 변경하는것이 좋음

### 이벤트 위임 (event delegation)

- 이벤트 위임을 이해하고, 제대로 쓸수 있도록 공부하기
- 이벤트 버블링에 대해서 이해도가 필요
- 부모 컨테이너는 어떤 자식 요소에서 이벤트가 발생하던, 모든 이벤트를 전부 들을 수 있음
- 가령, 다음과 같은 상황에서 li에 이벤트를 부여하고자 한다면

```tsx
<ul>
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
  <li>5</li>
  <li>6</li>
  <li>7</li>
  <li>8</li>
  <li>9</li>
  <li>10</li>
</ul>
```

- ul에 이벤트를 부여해서, event의 target이 li일 경우를 조건으로 잡아서 이벤트를 처리할 수 있음

### 쇼핑리스트 개선하기

- 쇼핑 목록 앱의 삭제 버튼에 일일히 이벤트를 부여했는데, 그렇게 하지 말고, 부모에만 부여하는 식으로 변경해보자
