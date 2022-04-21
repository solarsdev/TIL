# 유용한 객체들

## 빌트인 객체란 무엇인가?

- 자바스크립트를 브라우저나 노드에서 실행하면 코드가 런타임 환경에서 동작됨
- 자바스크립트안에는 다양한 내장객체들이 준비되어 있음
- 내장객체로는 파일에 직접 접근해서 자료를 송신하거나 할 수는 없고, 그런 작업이 필요하다면 호스트 객체가 필요함
  - 브라우저에서의 호스트 객체는 browser API와 같은 것들
  - Node환경에서는 Node에서 제공해주는 API와 같은 것들
- 빌트인되어 있는 객체 외에도 사용자가 직접 객체를 정의할수 있으며, 이를 사용자 정의 객체라고 표현함 (User-defined Objects)

## 래퍼 객체

```jsx
// 데이터의 종류에는 여러가지가 있으며, number는 숫자타입으로 원시 데이터 형태임
const number = 123;
console.log(number.toString());
// 원시타입은 데이터인데, 객체처럼 다양한 함수를 사용할수 있게 되어 있는건 왜일까?

// 래퍼 객체 (Wrapper Object)
// 원시값을 관련된 빌트인 객체로 변환해줌
// 언제 변환? : 필요에 의해
const num = 123; // 이 타이밍에는 원시 타입
// number. .을 사용하는 순간 래퍼객체에 의해 빌트인 객체로 변환됨
console.log(num); // 이 타이밍에는 다시 원시 타입으로 사용됨

const text = 'text';
console.log(text); // 역시 이 타이밍에는 원시 타입이지만
text.length; // 이때는 래퍼 객체에 의해 빌트인 string객체로 전환됨
text.trim(); // 이때는 래퍼 객체에 의해 빌트인 string객체로 전환됨
```

### 더 알아보기

- 런타임에 굳이 원시타입과 그것을 감싸는 래퍼객체를 사용해서 리얼타임으로 변환해가면서 써야 할 이유가 있을까?
- 어차피 객체로 표현이 가능하다면 원시타입이 아니라 처음부터 객체를 이용해서 처리하면 되지 않을까?
- 빌트인 객체의 경우에는 값 뿐만 아니라 데이터를 이용해서 처리할 수 있는 다양항 메소드들도 포함이 되어 있는 형태이기 때문에, 원시 타입 데이터에 비해 너무나 많은 메모리를 차지하게 됨 → 어플리케이션이 무거워짐

## 글로벌 객체

```jsx
console.log(globalThis);
console.log(this); // node는 browser와는 다름, browser는 this가 전역 객체임 (this = globalThis in browser)
console.log(Infinity);
console.log(NaN);
console.log(undefined);
//----

eval('const num = 2; console.log(num);');
console.log(isFinite(1));
console.log(isFinite(Infinity));

console.log(parseFloat('12.34'));
console.log(parseInt('12.34'));
console.log(parseInt('11'));

// URL (URI, Uniform Resource Identifier 하위 개념)
// 아스키 문자로만 구성되어야 함
// 한글이나 특수문자는 이스케이프 처리 해야 한다
const URL = 'https://구글.com';
const encoded = encodeURI(URL);
console.log(encoded);

const decoded = decodeURI(encoded);
console.log(decoded);

// 전체 URL이 아니라 부분적인 것은 Component 이용
const part = '구글.com';
console.log(encodeURIComponent(part));
```

## 불리언 함수들

[Boolean - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)

```jsx
// Boolean
const isTrue = new Boolean(true);
console.log(isTrue.valueOf());

/**
 * Falshy
 * 0
 * -0
 * null
 * NaN
 * undefined
 * ''
 */

/**
 * Truthy
 * 1
 * -1
 * '0'
 * 'false'
 * []
 * {}
 */
```

## 숫자 함수들

[Number - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)

```jsx
const num1 = 123;
const num2 = new Number(123);
console.log(typeof num1);
console.log(typeof num2);

console.log(Number.MAX_VALUE);
console.log(Number.MIN_VALUE);
console.log(Number.MAX_SAFE_INTEGER);
console.log(Number.MIN_SAFE_INTEGER);
console.log(Number.NaN);
console.log(Number.NEGATIVE_INFINITY);
console.log(Number.POSITIVE_INFINITY);

if (num1 === Number.NaN) {
}
if (Number.isNaN(num1)) {
}

// 지수표기법 (매우 크거나 작은 숫자를 표기할때 사용, 10의 n승으로 표기)
const num3 = 102;
console.log(num3.toExponential());

// 반올림하여 문자열로 변환
const num4 = 1234.12;
console.log(num4.toFixed());
console.log(num4.toString());
console.log(num4.toLocaleString('ar-EG'));

// 원하는 자릿수까지 유효하도록 반올림
console.log(num4.toPrecision(6));

if (Number.EPSILON > 0 && Number.EPSILON < 1) {
  console.log(Number.EPSILON);
}

const num = 0.1 + 0.2 - 0.2;
console.log(num); // 정확하게 부동소수점 계산이 되지 않음, 이를 나타내는것이 앱실론
// 계산상에서는 십진수를 이진수로 변경해서 계산 후에 다시 십진수로 반환함

function isEqual(original, expected) {
  return original === expected;
}

console.log(isEqual(1, 1));
console.log(isEqual(0.1, 0.1));
console.log(isEqual(num, 0.1)); // false

// 개량판
function isEqualImproved(original, expected) {
  return Math.abs(original - expected) < Number.EPSILON;
}

console.log(isEqualImproved(num, 0.1)); // false
```

## 수학 관련 함수들

[Math - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

```jsx
// Math
// static properties, method
console.log(Math.E); // 오일러의 상수, 자연로그의 밑
console.log(Math.PI); // 원주율 PI값

// static method
// 절대값
console.log(Math.abs(-10));
// 소수점 이하를 올림
console.log(Math.ceil(1.4));
// 소수점 이하를 내림
console.log(Math.floor(1.4));
// 소수점 이하를 반올림
console.log(Math.round(1.4));
console.log(Math.round(1.7));
// 정수만 반환
console.log(Math.trunc(1.7123));

// 최대, 최소값을 찾기
console.log(Math.max(1, 2, 3, 4));
console.log(Math.min(1, 2, 3, 4));

// 거듭제곱
console.log(3 ** 2);
console.log(Math.pow(3, 2));

// 제곱근
console.log(Math.sqrt(9));

// 랜덤한 값을 반환
console.log(Math.random());

// 1~10
console.log(Math.floor(Math.random() * 10 + 1));
```

## 문자열 함수들 ⭐️

[String - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

```jsx
const textObj = new String('Hello World!');
const text = 'Hello World!';
console.log(textObj);
console.log(text);

console.log(text[0]);
console.log(text[11]);
console.log(textObj.charAt(0));
console.log(textObj.charAt(11));

console.log(textObj.indexOf('o'));
console.log(textObj.lastIndexOf('o'));

console.log(textObj.includes('a'));
console.log(textObj.includes('H'));

console.log(textObj.startsWith('H'));
console.log(textObj.startsWith('!'));

const space = '                space              ';
console.log(space.trim());

const longText = 'Get to the point';
console.log(longText.split(' '));
```

## 날짜 관련 함수들

[Date - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

```jsx
console.log(new Date());
console.log(new Date('2022/04/01'));

// UTC (협정 세계시, 1970년 1월 1일 UTC 자정과의 시간차이를 밀리초 단위로 표기)
console.log(Date.now());

const now = new Date();
now.setFullYear(2023);
now.setMonth(0); // 1월
now.getDate(); // 0은 1일
now.getDay(); // 0은 월요일

console.log(now.toString());
console.log(now.toDateString());
console.log(now.toTimeString());
console.log(now.toLocaleDateString('ja-JP'));
```

## 퀴즈

```jsx
// 퀴즈!
// 1. 문자열의 모든 캐럭터를 하나씩 출력하라
const text = 'Hello World!';
text.split('').forEach((char) => {
  console.log(char);
});

// 2. 사용자들의 id를 잘라내어 각각의 id를 배열로 보관
const ids = 'user1, user2, user3, user4';
// ['user1', 'user2', 'user3', 'user4']
console.log(ids.split(', '));

// 3. 1초에 한번씩 시계를 (날짜포함) 출력해보자
setInterval(() => {
  console.log(new Date().toLocaleString('ja-JP'));
}, 1000);
```
