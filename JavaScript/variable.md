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
