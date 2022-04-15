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

[switch - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/switch)

```jsx
// 조건문 Conditional Statement
// switch
// if else if else if else if ... else
// 정해진 범위안의 값에 대해 특정한 일을 해야 하는 경우
let day = 6; // 0: 월요일, 1: 화요일... 6:일요일
let dayName;
if (day === 0) {
  dayName = '월요일';
} else if (day === 1) {
  dayName = '화요일';
} else if (day === 2) {
  dayName = '수요일';
} else if (day === 3) {
  dayName = '목요일';
} else if (day === 4) {
  dayName = '금요일';
} else if (day === 5) {
  dayName = '토요일';
} else if (day === 6) {
  dayName = '일요일';
}

// switch
switch (day) {
  case 0:
    dayName = '월요일';
    break;
  case 1:
    dayName = '화요일';
    break;
  case 2:
    dayName = '수요일';
    break;
  case 3:
    dayName = '목요일';
    break;
  case 4:
    dayName = '금요일';
    break;
  case 5:
    dayName = '토요일';
    break;
  case 6:
    dayName = '일요일';
    break;
  default:
    dayName = '그런 요일은 없음';
    break;
}

// switch문은 break를 만날때까지 수행하기 때문에, 여러 케이스의 처리가 동일하면 break를 사용하지 않음
let condition = 'okay'; // okay, good이면 좋음, bad이면 나쁨
switch (condition) {
  case 'okay':
  case 'good':
    console.log('좋음');
    break;
  case 'bad':
    console.log('나쁨');
    break;
}
```

## 반복문 for

[for - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for)

```jsx
// 반복문 Loop Statement
// for (변수선언문; 조건식; 증감식) { }
// 실행순서:
// 1. 변수선언문
// 2. 조건식의 값이 참이면 { } 코드블럭을 수행
// 3. 증감식을 수행
// 4. 조건식이 거짓이 될때까지 2번과 3번을 반복함

for (let i = 0; i < 5; i++) {
  console.log(i);
}

// 중첩 for문
for (let i = 0; i < 5; i++) {
  for (let j = 0; j < 5; j++) {
    console.log(i, j);
  }
}

// 무한루프
//for (;;) {
//  console.log('😜');
//}

// 반복문 제어: continue, break;
for (let i = 0; i < 20; i++) {
  if (i === 10) {
    console.log('i가 10이 되었음!');
    break; // 정지
    continue; // 패스
  }
  console.log(i);
}
```

## 반복문 while

[while - JavaScript | MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/while)

```jsx
// while(조건) { }
// 조건이 false가 될때까지 { } 코드블럭을 반복 실행
let num = 5;
while (num >= 0) {
  console.log(num);
  num--;
}

let isActive = true;
let i = 0;
while (isActive) {
  console.log('is alive');
  if (i === 1000) {
    break;
  }
  i++;
}

isActive = false;
do {
  console.log('is alive!');
} while (isActive);
```

## 제어문에서 많이 사용되는 연산자

```jsx
// 논리연산자 Logical operator
// && 그리고
// || 또는
// ! 부정 (단항연산자에서 온것)
// !! 불리언 값으로 변환 (단항연산자 응용버전)
let num = 8;

// and 연산자
if (num >= 0 && num < 9) {
  console.log('👍');
}

// or 연산자
if (num >= 0 || num < 9) {
  console.log('👍');
}
```
