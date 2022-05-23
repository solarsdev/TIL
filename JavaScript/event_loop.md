# Event Loop

## Event Loop에 대해서 어떤걸 배워야 하는지

- 프로세스와 쓰레드의 차이점
- 자바스크립트는 싱글 쓰레드인지 멀티 쓰레딩이 가능한지?
- 자바스크립트가 브라우저 위에서 어떻게 동작하는지
- 애니메이션을 줄 때 사용자 눈에 부드럽게 보이기 위해 60 frames per second 원칙에 대해서
- Task Queue, Microtask Queue의 차이점

## 프로세스와 쓰레드란 무엇인가?

### 프로세스

- 컴퓨터 (OS) 위에서 연속적으로 실행되고 있는 프로그램
  - 예를 들면, 사진을 보는 프로그램, 워드, 엑셀 등등
- 프로세스는 독립적으로 메모리 위에서 실행됨
- 프로세스에서 어떤 문제가 생긴다면 해당 프로세스만 종료됨 (강제 종료) → process is dead
- 각각의 프로세스는 저마다 자원이 정해져 있음
  1. 프로세스는 프로그램을 위한 코드가 들어있고
  2. 함수들이 어떤 순서로 실행되어야 하는지 들어있는 스택
  3. 오브젝트와 데이터를 생성할때 저장되는 공간인 힙
     - 힙에는 동적으로 할당되는 변수들이 할당됨
     - 데이터에는 전역 변수나 스태틱 변수들이 할당됨

### 쓰레드

- 프로세스 내부에서 작업 일부가 발생할 경우 처리하는 단위 (일꾼)
- 쓰레드들은 각각 처리해야 하는 함수의 호출 순서를 기억해야 하기 때문에 스택을 보유하고 있음
- 각각의 쓰레드들은 상호간의 처리해야 하는 내용 자체는 같기 때문에 아래와 같은 속성은 공통적으로 가지고 있음
  - 코드
  - 힙
  - 데이터
- 예를 들면, 동영상을 편집하는 프로세스에서
  1. 사진을 불러오고
  2. 음악을 불러오고
  3. 인코딩을 하는 등
- 각각의 작업이 발생하는데 해당 과정들을 복수의 쓰레드가 존재한다면 각각의 쓰레드들이 동시다발적으로 수행할 수 있음 → 프로세스의 효율화
- 만약, 쓰레드가 한개만 있다고 하면, 사진을 불러오는 동안 음악을 불러올 수 없는 등 한번에 하나의 일만 해야 함
- 쓰레드는 자신들이 일을 수행할 때 어디에서부터 어디까지 일을 했고 다음에는 어디인지 스택을 가지고 있지만, 공통적인 데이터 리소스는 프로세스에 있기 때문에 프로세스에 공통된 자원의 경우에는 동시다발적으로 접근하여 업데이트 하게 되는데, 멀티 쓰레딩이 어려운 이유가 여기에 있음

## 자바스크립트 런타임 환경 (엔진 이해, 스택 개념 정리)

- Java의 경우에는 언어 자체에서 멀티 쓰레딩이 지원됨
  - 프로그래머가 직접 특정 작업단위로 다른 쓰레드가 일을 하도록 지정할 수 있음
  - 총 몇개의 쓰레드가 동작할 수 있는지 지정 가능
- JavaScript의 경우에는 Single Threaded 언어
  - 언어 자체에서는 특정 쓰레드를 지정할 수 있는 방법이 없음
  - 단, JavaScript가 동작하고 있는 브라우저는 멀티 쓰레딩이 가능함 (Web APIs)
  - JavaScript가 동작하고 있는 Runtime Environment에서는 다른 방식을 이용해서 멀티 쓰레딩 효과를 얻을 수 있음
  - 뿐만 아니라 런타임에서는 이벤트 루프를 이용해서 다양한 동작을 실행할 수 있음
- JavaScript가 브라우저에서 동작하는 원리

  - JavaScript가 브라우저에 탑재되어 실행되면 JavaScript 엔진이 소스코드를 줄단위로 해석하고 실행함

    ```
    Memory Heap
    ----
    변수를 선언해서 오브젝트를 할당하거나, 문자열, 숫자를 할당하면 힙에 저장됨
    구조적으로 정리된 자료구조가 아니고, 아무곳이나 저장되어 있음

    Call Stack
    ----
    함수를 실행하는 순서에 따라서 차곡 차곡 LIFO (Last In First Out) 형태로 쌓임

    ==========
    second()
    first()
    main()
    ==========

    push, pop 등이 지원되는 자료구조
    ```

    ```jsx
    function second() {
      console.log('hello');
      return;
    }
    function first() {
      second();
      return;
    }
    function main() {
      first();
      return;
    }
    main();
    ```

## 브라우저 런타임 환경 이해 (매우 중요)

- 자바스크립트 런타임 환경만 가지고는 할 수 있는것이 한정적이고, Web APIs를 이용하면 할 수 있는것들이 많이 늘어남
- Web APIs는 브라우저에서 제공하는 각종 유용한 API들임

<aside>
💡 그렇다면, 자바스크립트 런타임 (single threaded) 콜스택 구조와 브라우저 (multi threaded) 환경에서는 어떻게 콜백함수를 등록해서 연계하는 걸까?

</aside>

- `setTimeout()` 을 수행했다고 가정

```jsx
function second() {
  setTimeout(() => console.log('hello!'), 1000);
  // setTimeout()을 실행하는 순간 자바스크립트의 런타임 콜스택에서는 setTimeout()이 바로 사라지고 Web APIs에서 타이머가 수행됨
  // 그렇게 되면 자바스크립트의 엔진은 바로 다음구문을 수행할수 있고 Web APIs가 작업을 수행하게 됨
  // Web APIs는 작업이 완료되면 Task Queue라는 곳에 콜백 자체를 집어넣게 됨
  // 상기 예에서는 1초가 지난 뒤에 arrow function을 Task Queue에 집어 넣음
  return;
}
function first() {
  second();
  return;
}
function main() {
  first();
  return;
}
main();
```

### Task Queue란 무엇인가?

- 큐도 자료구조의 일조인데 스택과 다르게 FIFO (First In First Out) 구조를 가진 형태를 말함
- 처음에 들어온 데이터가 처음으로 나감
- queue의 대표적인 수행으로는 add, remove

<aside>
💡 Task Queue에 들어온 콜백 함수는 언제 수행됨?

</aside>

- Task Queue를 관찰하는 아이가 바로 이벤트 루프 (Event Loop)
- while이나 for loop 처럼 계속해서 Call Stack과 Task Queue를 체크하도록 설계되어 있음
- 이벤트 루프의 우선순위로는 콜스택인데, 콜스택에 특정 일이 남아 있을 경우에는 콜스택에 있는 콜백을 먼저 수행하도록 설계되어 있음
- 콜스택이 비어서 자바스크립트 엔진이 아무일도 수행하고 있지 않을 때, Task Queue에 있는 콜백 함수를 콜스택에 가져와서 수행함
- Web APIs는 `setTimeout()` 외에도 다양한 API가 있는데, click 이벤트 등도 Task Queue에 포함됨
  - 여러번 버튼이 눌리면 Task Queue에 계속 쌓임
  - 콜스택이 비면 task queue를 콜스택으로 불러올 것임
  - Task Queue에서는 한번에 하나씩 콜스택으로 가져옴

## 큰 그림으로 이해하기 (`Microtask` queue)

- 콜스택에서 수행중인 함수는 코드블럭이 끝날때까지 수행이 보장되어 있음

### Microtask Queue

- 프로미스에 등록된 콜백 (then에 등록한 콜백함수)
- Mutation Observer에 등록된 콜백
- fetch를 이용해서 프로미스를 만들고 then으로 콜백함수를 등록해두면, resolve가 되었을때 등록된 콜백이 Microtask Queue에 들어오게 됨

### Render

- 브라우저에서 요소들에 애니메이션 효과를 추가하여 기동될때, 백로직상으로는 주기적으로 브라우저에 업데이트 해야 할 필요가 있음
- 화면에 업데이트 할때 Render가 호출됨
- 브라우저에서 DOM요소를 변형할때 화면에 반영되기 위해서는 Render Tree가 만들어져야 하고, Paint, Composite 과정을 통해서 표기됨
- Request Animation Frame을 통해 등록해두면, 브라우저 업데이트 전에 콜백을 수행하도록 등록하는 것임
  - 여기에 등록하게 되면 Render 이전에 큐 형태로 쌓이게 됨

<aside>
💡 지금까지 살펴본것만 해도 Task Queue, Microtask Queue, Render (Request Animation Frame Queue) 등 다양한 큐가 있는데, 브라우저에서는 어떻게 이런것들을 제대로 순서에 맞게 관리하는 걸까?

</aside>

- 이벤트 루프는 설계상으로 while (true) {} 의 형태를 이용해서 계속해서 확인하는 로직임
  - 이벤트 루프라는 이름이 붙은 이유!
- 이벤트 루프는 콜스택에서 수행중인 함수가 있다면 해당 포지션에서 이벤트 루프가 머물고 있음
  - 콜스택에서 시간이 오래걸리는 작업을 하게 되면 화면이 업데이트 되지 않는 이유!
  - 클릭을 해도 클릭에 걸린 콜백함수가 수행되지 않는 이유!
- 콜스택이 종료되고 다음으로는 Render를 체크 or not
  - 16.7ms동안 업데이트 해야 할 필요가 있음
    - 초당 60프레임을 표시해줘야 하기 때문
  - 그래서 이벤트 루프는 매번 Render를 체크하지는 않고 정해진 시간이 지나고 나면 한번 체크하러 감
- 다음으로는 Microtask Queue를 체크
  - 만약 Microtask Queue에 콜백함수가 밀려있다면 전부 콜스택에 차근 차근 넣어서 처리할때까지 멈춰있음
  - 수행 도중에 Microtask Queue에 콜백이 쌓이면 그 또한 빌때까지 처리하게 됨
- 마지막으로 Task Queue를 체크
  - Microtask Queue와의 차이점!
  - Task Queue에 있는 콜백함수는 1순회당 한개만 콜스택으로 가져온 뒤에 다음 루프로 진행

## 데모: Event Loop (add element)

```jsx
const button = document.querySelector('button');
button.addEventListener('click', () => {
  const element = document.createElement('h1');
  document.body.appendChild(element);
  element.style.color = 'red';
  element.innerText = 'Hello';
});
```

- 클릭 리스너의 콜백함수의 정의 순서
  1. h1 엘리먼트 생성
  2. body에 append
  3. 스타일 (컬러) 빨강으로 변경
  4. innerText를 Hello로 설정
- 상기 append부터 innerText까지의 속성설정에 순서가 상관없는 이유
  - 브라우저 Web APIs 상에서 클릭 이벤트가 발생하면 콜백함수가 Task Queue에 등록됨
  - Event Loop는 Task Queue를 확인하는 타이밍에 콜백 함수 전체를 콜스택에 가져와서 끝까지 실행
  - 실행이 종료되고 난 이후에 렌더링에 이동해서 브라우저 표기를 업데이트 하기 때문에 문제없이 작동됨
