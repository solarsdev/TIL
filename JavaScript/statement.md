# 제어문에 대해서

## 제어문을 공부해야 하는 이유?

- 제어문은 코드의 흐름을 제어하기 때문에 제어문이라고 불림
- 제어문의 종류
  - 조건문 (Conditional statement)
    - if
    - switch
    - 특정 조건일때만 수행하는 구문을 작성
  - 반복문
    - for
    - while
    - do-while
    - 특정 구문을 반복적으로 수행

## 조건문

### if...else

[if...else - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/if...else)

```jsx
// 조건문 Conditional Statement
// if(조건) { }
// if(조건) { } else { }
// if(조건) { } else if(조건) { } else { }
let fruit = 'apple';
if (fruit === 'apple') {
  console.log('🍎');
  let a = 1;
  console.log(a);
} else if (fruit === 'orange') {
  console.log('🍊');
} else {
  console.log('🍌');
}

if (false || 0 || '' || undefined || null) {
  console.log('출력되면 안됨!');
}
```

### 삼항 연산자 (깔끔👍)

[삼항 조건 연산자 - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)

```jsx
// 삼항 조건 연산자 Ternary Operator
// 조건식 ? 표현식1 : 표현식2
// 왼쪽의 조건식이 true이면 표현식1, false이면 표현식2
let fruit = 'apple';
fruit === 'apple' ? console.log('🍎') : console.log('🍌');

let emoji = fruit === 'apple' ? '🍎' : '🍌';
console.log(emoji);
```

## 막간 퀴즈

```jsx
// 퀴즈!!
let num = 2;
// num의 숫자가 짝수이면 👍 , 홀수라면 👎 을 출력하도록
// if
if (num % 2 === 0) {
  console.log('👍');
} else {
  console.log('👎');
}
// ternary
num % 2 === 0 ? console.log('👍') : console.log('👎');
```

## 언제 switch, 언제 if를 사용?
