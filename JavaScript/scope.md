# 스코프

## 스코프란?

[Scope - MDN Web Docs Glossary: Definitions of Web-related terms | MDN](https://developer.mozilla.org/en-US/docs/Glossary/Scope)

- Scope → 범위, 영역
- 프로그래밍에서는 변수를 `참조할 수 있는` 유효한 범위
- `식별자`가 유효한 범위
  - 변수, 함수, 클래스 이름
  - 선언된 위치에 따라 유효 범위가 결정됨
- 스코프는 블럭 안에서 정의되며, 블럭 안의 변수는 블럭 안에서만 유효
- 왜 스코프가 존재하나?
  - 이름 충돌을 방지
  - 메모리 관리 (블럭이 끝나면 변수가 정리됨)
- 결론 → 변수는 최대한 필요한 곳에서만 정의해야 함

```jsx
// 코드 블럭: {}, if() {}, for() {}, function() {}
// 블럭 외부에서는 블럭 내부의 변수를 참조할 수 ❌
{
  const a = 'a';
  // a가 유효한 범위는 블럭 안에서만
  console.log(a);
}
// console.log(a);
const b = 'b';

// 함수 외부에서는 함수 내부의 변수를 참조할 수 ❌
function print() {
  const message = 'Hello, world!';
  console.log(message);
}
// console.log(message);

// 함수 외부에서는 함수의 매개변수를 참조할 수 ❌
function sum(a, b) {
  console.log(a, b);
}
// console.log(a, b);
```

## 스코프 예제

```jsx
{
  const x = 1;
  {
    const y = 2;
    console.log(x);
  }
  console.log(x);
  // console.log(y); // 에러
}

const text = 'global'; // 전역 변수, 전역 스코프 (글로벌 변수, 글로벌 스코프)
{
  const text = 'inside block1'; // 지역 변수, 지역 스코프 (로컬 변수, 로컬 스코프)
  {
    console.log(text);
    // 가장 가까운 스코프가 먼저 참조됨
    // 같은 블럭 > 바로 위 부모 > ... > 글로벌 변수
  }
}
```

## 가비지 컬렉터 (Garbage Collector)

[Memory Management - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)

- 프로그래밍 언어인 C에서는 개발자가 직접 메모리 관리를 해야 함
- 메모리를 할당하고 정리하는 것 까지 개발자가 직접 해야 하기 때문에, 메모리 관리 여부에 따라서는 프로그램이 crash 할 수 있음
- C#, Go, Java, JavaScript의 경우 가비지 콜렉터가 존재하기 때문에 수동으로 해야 할 필요는 없음
- JavaScript에서 GC가 작동하는 원리
  ```jsx
  let apple = {
    name: 'apple',
  };
  let orange = apple;
  orange = null;
  apple = null;
  // orange, apple에 저장되어 있던 주소값이 제거되면
  // object 내용의 경우에는 불필요하기 때문에 GC가 파악되면 정리됨
  ```
- GC는 JavaScript 엔진의 백그라운드 프로세스로 동작됨
- GC의 작업 또한 CPU를 할당받아 처리하는 프로세스이므로, 너무 자주 실행되어도 곤란하고 여유시간에 실행해야 할 필요가 있음

```jsx
// 글로벌 변수는 앱이 종료될 때까지 메모리에 유지됨!
const global = 1;
{
  // 블럭 내부에서만 존재하고, 블럭이 끝나면 GC에 의해 정리됨
  const local = 1;
}
function print() {
  // 함수 내부에서도 블럭안에서 필요한 경우에는
  // 필요한 곳에서만 변수를 정의해서 사용하는 것이 메모리 측면에서 좋음
  if (true) {
    let temp = 0;
  }
}
```

## 렉시컬 환경이란?

### 실행 컨텍스트 Execution Context

- 코드의 실행 순서와 스코프를 기억하고 있음
- 하나의 싱글 컨텍스트 스택 (JavaScript)

```jsx
// 전역 스코프
const a = 1;
{
  // 블럭1 스코프
  const a = 2;
  {
    // 블럭2 스코프
    const a = 3;
  }
}
```

- 각각의 블럭은 {} 내부적으로 어떻게 구분되고, 부모임을 어떻게 정의하고, 어떻게 변수를 찾아낼까?
  - 각각의 블럭은 렉시컬 환경 (Lexical Environment)라는 내부 객체를 가지고 있음
  - 렉시컬 환경은 내부 변수가 어떤것들이 있는지, 부모가 누구인지 등 정보를 소유하고 있는 객체임
  - 블럭 = 함수, 일반 블럭 전부 포함됨
- 렉시컬 환경 (Lexical Environment)
  - 환경 레코드 (Environment Record)
    - 현재 블럭에 해당하는 정보를 담고 있음
  - 외부 환경 참조 (Outer Lexical Environment Reference)
    - 부모가 누구인지등에 해당하는 정보를 담고 있음
- 실행 컨텍스트 스택
  1. 전역 스코프 렉시컬 환경
     1. 전역 스코프 환경 레코드 → 전역 변수 정보
     2. 전역 스코프 외부 환경 참조 → null
  2. 블럭1 스코프
     1. 블럭1 스코프 환경 레코드 → 블럭1 변수 정보
     2. 블럭1 스코프 외부 환경 참조 → 전역 렉시컬 환경 (스코프 체인)
  3. 블럭2 스코프
     1. 블럭2 스코프 환경 레코드 → 블럭2 변수 정보
     2. 블럭2 스코프 외부 환경 참조 → 블럭1 렉시컬 환경 (스코프 체인)
  - 블럭이 종료되면 GC는 스택에서 해당 블럭에 해당하는 내용을 정리함
  - 블럭2 스코프의 환경 레코드에 존재하지 않는 변수를 참조할 경우 흐름
    1. 환경 레코드에 존재하지 않음을 확인
    2. 스코프 체인을 통해서 부모의 렉시컬 환경 레코드에서 확인
    3. ...
    4. 전역 렉시컬 환경까지 진행함
  - 이러한 과정을 거치기 때문에, 메모리 측면 뿐만 아니라 `퍼포먼스의 개선을 위해서라도` 최대한 필요한 곳에서만 정의하는 것이 필요

## 호이스팅 정리

- 호이스팅 → 끌어 올리다
- JavaScript Engine(번역기, 인터프리터)이 `코드를 실행하기 전`에 `변수, 함수, 클래스의 선언`을 제일 위로 끌어올리는 것
- 변수의 선언과 초기화를 분리한 뒤, `선언만 코드의 최상단`으로 옮김
- 변수의 선언문
  - let → 재할당이 반드시 필요한 경우
  - const → 재할당이 필요없을 경우, 혹은 기본적으로
  - var → ECMA Script 6 이전, 사용하지 말자

```jsx
// 함수의 호이스팅은 함수의 선언문 전에 호출이 가능하게 해줌
// 함수의 선언문은 선언 이전에도 호출이 가능함
print();

function print() {
  console.log('Hello');
}

// 변수 (let, const)와 클래스는 선언만 호이스팅이 되고, 초기화는 안됨
// 초기화 전, 변수에 접근하면 컴파일(빌드)에러가 발생함
// console.log(hi);
let hi = 'hi';
//func1();
let func1 = function () {};
// const cat = new Cat();
class Cat {}

let x = 1;
{
  console.log(x);
  // let x = 2;
  // 지역 렉시컬 환경이 우선시되기 때문에
  // let x의 선언부가 호이스팅됨
}
```

## var 변수에 대해서

```jsx
// var의 특징 💩
// -> 일반 코딩 방식과 어긋나서 개발하면서도 멘붕이 옴
// -> 코드의 가독성과 유지보수성에 좋지 않음

// 1. 변수 선언하는 키워드 없이 선언과 할당이 가능
// 선언인지 재할당인지 구분이 어려움
something = '💩';
console.log(something);

// 2. 중복 선언이 가능함
// 협업과정에서 나도 모르게 재할당을 해버릴 가능성이 있음
var poo = '💩';
var poo = '💩';
var poo = '💩';
console.log(poo);

// 3. 블록 레벨 스코프 안됨 ❌
var apple = '사과';
{
  var apple = '🍎';
  {
    var apple = '🍏';
  }
}
console.log(apple);

// 4. 함수 레벨 스코프는 지원 됨
function example() {
  var dog = '🐶';
}
//console.log(dog);
```

## 엄격 모드

[Strict mode - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)

```jsx
'use strict';
// 엄격 모드 strict mode
// 리액트와 같은 프레임워크 사용시 기본적으로 엄격 모드임
// var x = 1;
// delete x;

function add(x) {
  var a = 2;
  var b = a + x;
  console.log(this);
}
add(1);

const array = [1, 2, 3];
//for (num of array) {
//  console.log(num);
//}
```
