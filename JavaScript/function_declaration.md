# Arrow Function은 무엇인가? 함수의 선언과 표현

[자바스크립트 5. Arrow Function은 무엇인가? 함수의 선언과 표현 | 프론트엔드 개발자 입문편(JavaScript ES6)](https://youtu.be/e_lU39U-5bQ)

- 프로그램 내에는 저마다 기능을 수행하는 함수들이 존재
- 절차지향적인 언어의 경우에는 함수가 프로그램에서 중요한 역할을 담당
- 자바스크립트에는 class가 추가되었으니 object oriented language가 아닌가?
  - 자바스크립트에 클래스가 추가되었다고 하더라도 프로토타입을 베이스로 한 가짜 오브젝트임
  - 따라서 자바스크립트도 절차지향적인 언어라고 볼 수 있음
- 함수는 또 다른 말로 sub-program이라고도 부름
  - 프로그램 안에서 각각의 다른 기능을 수행하기 때문
- 함수는 input을 처리해서 output을 내놓는 것임
- 언어 자체에 존재하는 API를 사용할때, 함수의 이름을 보고 기능을 유추하게 됨
  - 따라서, 함수에서는 input과 output (인터페이스)가 중요
  - 이름또한 그 함수의 기능을 어느정도 표현할 수 있으니 잘 짓는것이 중요
- 자바스크립트에서 함수를 정의하는 방법

```jsx
function name(param1, param2) { body ... return; }
// 하나의 함수는 한가지의 일만 하도록 하자
// naming: doSomething, command, verb (동사의 형태로 이름을 지정)
// 함수의 이름을 짓기가 어렵다면, 혹시 함수 내에서 다양한 기능을 하고 있는게 아닐까 다시 생각
// 세분화해서 다시 나눌수 있지는 않을지 고민해볼 필요가 있음
// 자바스크립트에서 함수는 오브젝트임 * 이러한 특성으로 함수를 변수에 담을 수 있음
```

## Parameter

- primitive 매개변수의 경우에는 값을 전달
- object 매개변수의 경우에는 ref값을 전달 (참조 주소같은 개념)

```jsx
function changeName(obj) {
  obj.name = 'coder';
}
const ellie = { name: 'ellie' };
changeName(ellie);
console.log(ellie);
// output: {name: "coder"}
// object 매개변수가 값을 전달했다면 ellie는 바뀌지 않았겠지만, ref값을 전달하고 ellie object를
// 참조하여 수정하였기 때문에 값이 변화한것을 볼 수 있음
```

## Default parameters (ES6에서 추가됨)

- 매개변수의 기본값을 지정하는 기능

```jsx
function showMessage(message, from) {
  console.log(`${message} from ${from}`);
}
showMessage('Hi');
// output: Hi from undefined

function showMessage(message, from = 'unknown') {
  console.log(`${message} from ${from}`);
}
showMessage('Hi');
// output: Hi from unknown
```

## Rest parameters

- ...을 작성하게 되면 매개변수로 전달된 값들이 array로 합쳐져서 전달됨

```jsx
function printAll(...args) {
  for (let i = 0; i < args.length; i++) {
    console.log(args[i]);
  }
}
printAll('dream', 'coding', 'ellie');
```

- Rest parameters의 주의점
  - Rest parameters는 매개변수의 제일 뒤에 와야함
  - 동시에 2개 이상의 Rest parameters를 이용할 수 없음
- Rest parameters의 등장 배경

  - 나머지 연산자의 경우, 기존 자바스크립트에서 매개변수를 명시해야 하기 때문에 발생하는 추가적인 코드를 줄일 수 있도록 도와줌

  ```jsx
  // Before rest parameters, "arguments" could be converted to a normal array using:

  function f(a, b) {
    let normalArray = Array.prototype.slice.call(arguments);
    // -- or --
    let normalArray = [].slice.call(arguments);
    // -- or --
    let normalArray = Array.from(arguments);

    let first = normalArray.shift(); // OK, gives the first argument
    let first = arguments.shift(); // ERROR (arguments is not a normal array)
  }

  // Now, you can easily gain access to a normal array using a rest parameter

  function f(...args) {
    let normalArray = args;
    let first = normalArray.shift(); // OK, gives the first argument
  }
  ```

  - 나머지 연산자가 도입되기 전에는 arguments라는 오브젝트가 존재했음
    - arguments는 가상의 배열로서 함수에 입력된 매개변수들의 집합을 말함
    - 어디까지나 가상의 배열이기 때문에 배열에 사용할수 있는 배열함수를 직접 사용하는것이 불가능
    - 배열로 변환해야 하기 때문에, 매개변수 배열을 활용하기 위해서 필수적으로 사용하는 코드들이 존재했음 (보일러 플레이트)
    - 나머지 연산자로 반환되는 배열의 경우에는 array-like가 아닌 실제 배열이기 때문에 바로 배열 함수를 사용할수 있음

<aside>
💡 자바스크립트의 연산자인 Spread syntax (...) 와는 다른 개념임에 주의하자

</aside>

## Local Scope 🙌

- Block Scope, Global Scope와 같이 지역 범위도 존재함
- 밖에서는 안이 보이지 않고, 안에서만 밖을 볼 수 있음
- 블럭 안에서 선언한 변수는 블럭 밖에서 접근이 불가
- 자식은 부모에 정의된 변수, 함수를 사용할 수 있지만, 부모는 자식 내에 정의되어 있는 변수, 함수를 사용할 수 없음

<aside>
💡 클로저에 대해서 나중에 조사

</aside>

## Return

- 함수에서는 인풋을 받아서 연산 후에 return을 통해서 아웃풋으로 내보낼 수 있음

### Early Return 🙌

- 현업에서 사용되는 팁
- early return 혹은 early exit을 수행해라
- 특정 조건 하에서 로직을 수행하는데, 해당 로직이 길어질 경우에 가독성을 해치게 됨

  ```jsx
  function upgradeUser(user) {
    if (user.point > 10) {
      // long upgrade logic...😭
    }
  }

  // 이럴땐 이렇게 ealry return으로 처리하면 가독성이 상승
  function upgradeUser(user) {
    if (user.point <= 10) {
      return;
    }
    // long upgrade logic...😘
  }
  ```

## Function Expression

- First-Class function
  - 함수지만 값으로서 변수에 할당 가능
  - 다른 함수의 매개변수로 사용 가능 (변수를 통해서)
  - 다른 함수의 리턴값으로도 사용 가능
  - 다만 함수 자체가 아니고 함수에 대한 레퍼런스값을 이용한 방식
  ```jsx
  const print = function () {
    // 익명 함수
    console.log('print');
  };
  // 익명 함수를 print 변수에 담음
  ```
  - 익명 함수를 변수에 할당하는것과, 함수에 이름을 지어주는것의 차이점이 뭘까?
    - 익명 함수를 변수에 할당한 뒤부터 변수명을 이용해서 함수를 사용할수 있음
    - 함수에 이름을 지어주면 해당 함수는 hoisted되어 가장 윗단으로 올라가므로, 코드상에서는 함수 선언 이전에 함수를 사용해도 사용할수 있음

## Callback Function 😓

- 콜백이란?

  ```jsx
  function randomQuiz(answer, printYes, printNo) {
    if (answer === 'love you') {
      printYes();
    } else {
      printNo();
    }
  }

  const printYes = function () {
    console.log('Yes!');
  };

  const printNo = function print() {
    console.log('No!');
  };

  randomQuiz('wrong', printYes, printNo);
  randomQuiz('love you', printYes, printNo);
  ```

  - 이때 전달된 printYes, printNo가 바로 콜백 함수
  - 익명함수와 이름함수를 사용했는데, 이름지어진 함수를 사용하면 디버깅할때 이름이 표시되기 때문에 디버깅하기가 쉬워지며, 함수 내에서 재귀호출이 가능하기 때문에 필요하다면 이름을 지음

## IIFE: Immediately Invoked Function Expression

```jsx
(function hello() {
  console.log('IIFE');
})();
// 함수를 만들면서 바로 호출
```
