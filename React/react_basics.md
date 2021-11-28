## React Basics

### React를 사용하는것에서 오는 편리함

- UI를 Interactive하게 변경함
- JavaScript vs React 양쪽으로 코딩해보자

  - 버튼을 누르면 텍스트에는 몇번 클릭했는지 표시해주는 웹 어플리케이션

    - 바닐라 자바스크립트 버전
      ```html
      <button id="btn">Click Me</button> <span>Total Clicks: 0</span>
      ```
      ```jsx
      let counter = 0;
      function handleClick() {
        counter += 1;
        const span = document.querySelector('span');
        span.innerText = `Total Clicks: ${counter}`;
      }
      const btn = document.getElementById('btn');
      btn.addEventListener('click', handleClick);
      ```
    - 리액트 버전 (createElement 방식을 이용)

      ```jsx
      let counter = 0;
      let span = React.createElement('span', null, `Total Clicks: ${counter}`);

      const btn = React.createElement(
        'button',
        {
          onClick: () => {
            counter++;
            span = React.createElement('span', null, `Total Clicks: ${counter}`);
            ReactDOM.render([btn, span], body);
          },
        },
        `Click Me`,
      );

      const body = document.querySelector('body');
      ReactDOM.render([btn, span], body);
      ```

        <aside>
        💡 이걸로만 보면 딱히 React가 코드를 줄인다고 표현하긴 그렇고, createElement에서 요소의 컨텐츠와 이벤트 핸들링을 정의된 프로퍼티로 이용할수 있다는점은 분명 좋은점이 있다고 판단됨
        
        </aside>

### JSX란?

- JSX란 JavaScript를 확장한 문법을 말함
- JavaScript문법으로 엘리먼트를 작성할때 HTML태그를 그대로 사용 가능함

  - 리액트 + JSX버전 (babel을 추가해서 하드코딩 스크립트를 브라우저에 맞게 변환 추가)

    ```jsx
    let counter = 0;
    let span = <span>Total Clicks: {counter}</span>;

    const btn = (
      <button
        onClick={() => {
          counter++;
          span = <span>Total Clicks: {counter}</span>;
          ReactDOM.render([btn, span], body);
        }}>
        Click Me
      </button>
    );
    const body = document.querySelector('body');
    ReactDOM.render([btn, span], body);
    ```

      <aside>
      💡 확실히 JSX를 이용하면, HTML문법을 그대로 사용할 수 있기 때문에 가독성이 좋아질 뿐더러, 리액트의 장점인 컨텐츠와 이벤트 핸들링을 하나의 요소에서 전부 정의할수 있다는점에서 양쪽의 장점만 섞어서 코딩이 가능할것으로 보임
      
      </aside>
      
      <aside>
      💡 그럼 counter같은 변수를 표현하기 위해서는 항상 컴포넌트를 매번 새로 정의해서 새롭게 렌더링을 해줘야 할까? 이건 React에서 분명 편의제공을 해주는 부분이 있어보임
      
      </aside>

  - 바벨을 임포트, 하드코딩 방식으로 사용하기 위해서 두줄이 추가됨
    ```jsx
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script type="text/babel"></script>
    ```
  - 대문자로 컴포넌트를 정하는 이유에 대해서
    - JSX에서는 HTML 태그를 그대로 이용하는 방식을 사용하고 있기 때문에, 컴포넌트명을 대문자 시작으로 하지 않게 되면 HTML 태그의 정의어와 충돌할 수 있기 때문

### JSX로 코딩할때 주의점

- JSX에서는 HTML태그를 그대로 사용할수 있지만, 기본적으로는 JavaScript 규약이므로 주의해야 할 점들이 있음
- 프로그래밍 언어에서는 예약된 구문들이 존재하는데, JavaScript에서 for나 class같은 것들임
- 이는 HTML에서 어떤 역할을 동시에 수행하는데, 예를들어 다음의 코드를 확인해보자
  - 일반적인 HTML 태그로 작성된 코드
  ```jsx
  <label for='idHere'></label>
  <input id='idHere' class='classNameHere' type='text' />
  ```
  - JSX에서는 이렇게 바뀜
  ```jsx
  <label htmlFor='idHere'></label>
  <input id='idHere' className='classNameHere' type='text' />
  // 어디까지나 JSX이므로 for나 class는 예약어로 사용됨
  // for는 htmlFor, class는 className으로 대체해서 사용해야 함
  ```
