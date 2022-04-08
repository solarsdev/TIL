# 변수에 대해서

## 컴퓨터의 구성요소 (메모리)

- 컴퓨터의 저장장치
  - 하드디스크
  - CPU (연산)
  - 메모리
- 컴퓨터에서 프로그램을 실행하는 방법
  - 하드디스크에 저장된 프로그램이나 코드를 실행하기 위해서는 먼저 메모리에 관련 내용을 적재해야 함
  - 메모리에 적재된 내용을 바탕으로 CPU가 연산을 수행
  - 프로그램을 종료하기 전 메모리에 변경된 내용을 하드디스크에 다시 저장함
- 메모리의 생김새
  - 메모리는 여러개의 셀이 묶여있는 형태
  - 각각의 메모리 셀은 1바이트 사이즈로 존재하는 연속된 공간임
  - 어플리케이션을 열때마다 메모리에 필요한 내용을 적재하게 됨
  - 어플리케이션이 늘어나서 메모리 공간이 가득차게 되면 어떻게 될까?
    - 사용하지 않는 어플리케이션에 관련된 내용을 하드디스크에 잠시 저장
    - 필요한 만큼 메모리에 할당해서 사용
    - 총 메모리의 양보다 더 많은 메모리를 요구하게 된다면? 어플리케이션이 정상적으로 작동하지 않게 됨
  - 어플리케이션이 메모리에 적재됐을때 상태
    - Code (개발자가 작성한 코드)
    - Data (변수)
    - Stack (함수등의 실행 순서)
    - Heap (객체들을 저장하는 공간)

## 변수 선언 및 할당

- 어플리케이션을 실행하면 다음과 같은 순서로 동작됨
  1. 입력 (사용자의 클릭, 값 입력 등)
  2. 처리 (엔진에서 처리)
  3. 출력 (저장 or 송신)
- 데이터를 처리하는것이 실질적인 프로그램의 동작이라고 할 수 있는데, 이때 입력받는 데이터를 보관하는 변수라고 하고 변수는 `이름이 주어진 기억장소`를 말함

### 변수의 종류

- let

  ```jsx
  let a = 0; // 선언과 동시에 변수에 값을 할당
  a = 1; // 값의 재할당
  ```

  - let를 이용해서 변수에 데이터를 저장하면 메모리의 특정 위치에 데이터가 할당됨
    - 0x000006
  - 메모리 위치를 직접 지정하는 방식이 되어버리면 기억하기 어렵기 때문에, 임의의 이름으로 대신함

  ```jsx
  let a = 0;
  console.log(a);

  a = 1;
  console.log(a);

  let b;
  console.log(b);

  b = 2;
  console.log(b);
  ```

## 변수 이름 짓는 법

- Naming Variables
  - 값을 저장하는 공간
  - `저장된 값을 잘 나타낼 수 있는 의미 있는 이름`
  - 구체적일 수록 좋음
  - 항상 “이 변수에는 어떤 값이 들어 있는가?” 를 고민해보자
    [Storing the information you need - Variables - Learn web development | MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/Variables)
- 자바스크립트에서 기본적으로 사용하고 있는 키워드 (변수로 이름지어줄수 없음)
  [Lexical grammar - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#keywords)

```jsx
/** 변수 규칙
 * 라틴문자 (0-9, a-z, A-Z), _
 * 대소문자를 구별함
 * 추천: camelCase (likeThis) ✅
 * 한국어 ❌
 * 예약어 ❌
 * 숫자로 시작 ❌
 * 특수문자 ❌ (_, $ 두가지는 예외)
 * 이모지 ❌
 * 여러개의 변수를 숫자로 지정 ❌ -> 최대한 의미있게, 구체적인 이름으로 작성 ✅
 */

let apple;
let redApple;

// 나쁜 예제1 💩
let number = 20;

// 좋은 예제1 ✨
let myAge = 20;

// 나쁜 예제2 💩
let audio1;
let audio2;

// 좋은 예제2 ✨
let backgroundAudio;
let windAudio;

// 팁! 👍
let audioBackground;
let audioWind;
// 대분류를 먼저 변수로 정의해줌
// audio를 쳤을때 자연스럽게 하위 분류를 찾아낼 수 있음
```

## 데이터 타입

- 자바스크립트에서의 데이터 타입은 다양한 것들이 있음
- 원시타입 (primitive)
  - 단일 데이터를 담을 수 있는 공간
    - number
      [BigInt - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
      [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)
      ```jsx
      let integer = 123; // 정수
      let negative = -123; // 음수
      let double = 1.23; // 실수
      console.log(integer);
      console.log(negative);
      console.log(double);

      let binary = 0b1111011; // 2진수
      let octal = 0o173; // 8진수
      let hex = 0x7b; // 16진수
      console.log(binary);
      console.log(octal);
      console.log(hex);

      console.log(0 / 123); // 0
      console.log(123 / 0); // Infinity
      console.log(123 / -0); // -Infinity
      console.log(123 / 'text'); // NaN (Not a Number)

      let bigInt = 1234566789012345667890123456678901234566789012345667890n;
      console.log(bigInt);
      ```
    - string
      [String - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
      ```jsx
      // 문자열타입
      let string = '안녕하세요';
      string = `안녕!`;
      console.log(string);

      // 특수문자 출력하는법
      string = "'안녕!'";
      console.log(string);

      string = '안녕!\n내 이름은\t\t누굴까\\\u09AC';
      console.log(string);

      // 템플릿 리터럴 (Template Literal)
      let id = '테스트';
      let greetings = '안녕, ' + id + '👍 좋은 하루 보내!';
      console.log(greetings);

      greetings = `안녕, ${id}👍 좋은 하루 보내!`;
      console.log(greetings);
      ```
    - boolean
      [Boolean - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Boolean)
      ```jsx
      // 불리언 타입
      let good = true;
      let bad = false;
      console.log(good);
      console.log(bad);

      // 활용예
      let isFree = true;
      let isActivated = false;
      let isEnrolled = true;
      console.log(isFree);
      console.log(isActivated);
      console.log(isEnrolled);

      console.clear();
      // Falshy 거짓인 값
      console.log(!!0);
      console.log(!!-0);
      console.log(!!'');
      console.log(!!null);
      console.log(!!undefined);
      console.log(!!NaN);

      // Truthy 참인 값
      console.log(!!1);
      console.log(!!-1);
      console.log(!!'text');
      console.log(!!{});
      console.log(!!Infinity);
      ```
    - null / undefined
      ```jsx
      // null, undefined
      let variable;
      console.log(variable);

      variable = null;
      console.log(variable);

      // 변수에 아직 할당이 없다면 undefined
      // 값이 없다는것을 명시적으로 선언하는것이 null

      let activeItem; // 아직 활성화된 아이템이 있는지 없는지 모르는 상태
      activeItem = null; // 활성화된 아이템이 없는 상태

      console.log(typeof 123);
      console.log(typeof '123');

      console.log(typeof null); // 오브젝트
      console.log(typeof undefined);
      ```
    - Symbol
- 객체 (object)
  - 복합적인 데이터를 담을 수 있는 공간
    - object - array
    - function
  - 상태와 행동 등을 저장할 수 있음
  - 객체는 `{ key: value }`의 형태로 표현
    - value에는 원시 데이터나 객체가 또 한번 올 수 있음 recursive
    ```jsx
    {
    	id: 1234,
    	key: 'secret-key',
    }
    ```
    - 원시데이터는 메모리에 직접(Data, Stack) 데이터가 저장되는데 반해, 객체타입은 Heap에 저장됨
    ```jsx
    let apple = {
      name: 'apple',
      color: 'red',
      display: '🍎',
    };

    // apple은 메모리 셀 안에 주소값을 가지며
    // 힙 안에 있는 주소에 여러 셀 안에 각각의 실질 키 밸류가 저장됨
    ```
- 정리
  ```jsx
  let name = 'apple';
  let color = 'red';
  let display = '🍎';
  let orangeName = 'orange';

  let apple = {
    name: 'apple',
    color: 'red',
    display: '🍎',
  };

  console.log(apple);
  console.log(apple.name);
  console.log(apple.color);
  console.log(apple.display);

  let orange = {
    name: 'orange',
    color: 'orange',
    display: '🍊',
  };

  console.log(orange);
  console.log(orange.name);
  console.log(orange.color);
  console.log(orange.display);
  ```

## 값과 참조의 차이 (중요⚡️)

- 원시 타입에서는 실질적인 데이터가 저장되어 있기 때문에, 카피 작업이 데이터를 그대로 복사하는 것이 되지만 (Copy by Value), 객체 타입에서는 변수에는 실질적인 데이터가 아닌, 주소가 들어있기 때문에, 주소값이 복사됨 (Copy by Reference)
- 주소값이 복사되면 실질적인 데이터가 들어있는 동일한 힙을 가리키게 되므로 서로 다른 두 변수에서 같은 데이터 주소를 가리키게 됨
- 이를 shallow copy라고 부름

```jsx
// 원시타입은 값이 복사되어 전달됨
let a = 1;
let b = a; // 1
b = 2;
console.log(a);
console.log(b);

// 객체타입은 참조값(메모리 주소, 레퍼런스)가 복사되어 전달됨
let apple = {
  // 0x1234
  name: 'apple',
};
let orange = apple;
orange.name = 'orange';
console.log(apple);
```

## 상수 변수 const

```jsx
// let 재할당이 가능
let a = 1;
a = 2;

// const 재할당이 불가능
// 1. 상수
// 2. 상수변수 또는 변수
const text = 'hello';
//text = 'hi'; ❌

// 1. 상수
const MAX_FRUITS = 5;

// 2. 재할당 불가능한 상수변수 또는 변수
const apple = {
  name: 'apple',
  color: 'red',
  display: '🍎',
};
console.log(apple);
apple.name = 'orange';
apple.display = '🍊';
console.log(apple);

// 왜? apple이라는 변수 자체는 const이기 때문에 새로운 주소값을 덮어쓰는것은 안되지만,
// 그 안에 있는 name과 같은 키밸류는 다른 공간 (Heap) 에 할당되어 있는 데이터기 때문에 사용 가능
```

## 타입 확인 법 (typeof)

- 컴파일러
  - 컴파일러를 가지고 있는 언어는 정적 언어
  - 컴파일 시점에 타입을 정의함
- 인터프리터
  - 인터프리터를 가지고 있는 언어는 동적 언어
  - 실행시점에 타입을 정의함
- 자바스크립트
  ```jsx
  // typeof: 데이터 타입을 확인
  // 값을 타입 문자열로 반환
  let variable;
  console.log(typeof variable);

  variable = '';
  console.log(typeof variable);

  variable = 1;
  console.log(typeof variable);

  variable = false; // 실행 시점에 할당 데이터에 따라 타입이 결정됨
  console.log(typeof variable);

  variable = {};
  console.log(typeof variable);

  variable = function () {};
  console.log(typeof variable);

  variable = Symbol();
  console.log(typeof variable);
  ```
  - 자바스크립트에서도 타입이 있긴 함!
  - 다만, 실행시점에 할당값에 따라 동적으로 변화하는 Dynamic Variable
