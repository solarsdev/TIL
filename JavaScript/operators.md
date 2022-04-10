# 연산자에 대해서

## 연산자는 무엇인가?

- 어플리케이션에서 어떤 데이터를 처리할때 필요한 연산자

[JavaScript 참고서 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference)

## 표현식에 대해서

- 값 = 리터럴 (코드에서 `값을 나타내는 표기법`)
- 리터럴의 예시
  1. 숫자를 그냥 쓰면 number
  2. ‘’, “” 와 같이 따옴표를 쓰게 되면 string
  3. true 또는 false는 boolean
  4. {} 는 object
  5. [] 는 array
  6. `` (Template Literal)
     - 템플릿 리터럴이라고 부르는 이유
     - 문자열 사이에 ${} 등과 같이 정해진 템플릿을 이용해서 변수를 표현할 수 있기 때문
  7. function() {} function을 나타내기 위한 리터럴
- Statement

  - 반복문
  - 조건문
  - Expressions (표현식)

    - 값으로 평가될 수 있는 문장

    ```jsx
    1; // 숫자 리터럴 표현식
    1 + 1; // 연산자 표현식
    call(); // 함수 호출 표현식 (함수가 호출된 뒤 리턴값에 따라 값으로 표현될 수 있기 때문)
    let b; // 선언문
    b = 2; // 할당문, 할당 표현식인 문

    let b; // 선언문
    b = 2; // 표현식, 할당문

    //let a = let b; // 에러 ❌
    let a = (b = 2);
    console.log(a);
    ```

## 산술연산자 (arithmetic operators)

```jsx
// 산술 연산자 (Arithmetic operations)
// +
// -
// *
// /
// %
// ** 지수 (거듭제곱)

console.log(5 + 2);
console.log(5 - 2);
console.log(5 * 2);
console.log(5 / 2);
console.log(5 % 2);
console.log(5 ** 2); // es7
console.log(Math.pow(5, 2)); // 상동

// + 연산자 주의점!
let text = '두개의' + '문자를';
console.log(text);
text = '1' + 1; // 숫자와 문자열을 더하면 결과값이 문자열로 변환됨
console.log(text);
```

## 단항연산자 (unary operators)

```jsx
// 단항연산자 Unary Operators
// + (양)
// - (음)
// ! (부정)
let a = 5;
a = -a; // -1 * 5
console.log(a);

a = -a;
console.log(a);

a = -a;
console.log(a);

a = +a;
console.log(a);

let boolean = true;
console.log(boolean);
console.log(!boolean);
console.log(!!boolean);

// + 숫자가 아닌 타입들을 숫자로 변환하면 어떤 값이 나오는지 확인할 수 있음
console.clear();
console.log(+false);
console.log(+null);
console.log(+'');
console.log(+true);
console.log(+'text');
console.log(+undefined);
```
