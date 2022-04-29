# 주석, 에러처리, 모듈

## 주석처리 잘 하는법

[https://github.com/jsdoc/jsdoc](https://github.com/jsdoc/jsdoc)

```jsx
// 주석 Comments
// 한줄짜리 주석을 작성할때 씀
// ToDo: 해야 할 일을 작성
// ToDo(Lee): 어떤 기능을 구현하기

/**
 * 주석은 코드 자체를 설명하는 것이 아니라,
 * 왜(why)와 어떻게(how)를 설명하는 것이 좋음
 * 단, 정말 필요한 경우에만!
 */

// 외부에서 많이 쓰이는 함수 API인 경우 JSDoc을 사용하는 것이 좋음
// JSDoc wiki 참조

/**
 * 주어진 두 인자를 더한 값을 반환함
 * @param {*} a 숫자 1
 * @param {*} b 숫자 2
 * @returns a와 b를 더한값
 */
function add(a, b) {
  return a + b;
}

// 다른 라이브러리의 document를 참고해서 작성해볼것
```

## 에러 처리

```jsx
// try catch finally
function readFile(path) {
  // throw new Error('파일 경로를 찾을 수 없음');
  return '파일의 내용';
}

function processFile(path) {
  let content;
  try {
    readFile(path);
  } catch (error) {
    console.log(error);
    content = '기본내용';
  } finally {
    console.log('성공하든 실패하든 마지막으로 리소스를 정리할 수 있음!');
  }
  const result = 'hi ' + content;
  return result;
}

const result = processFile('경로');
console.log(result);
```

```jsx
// Bubbling up, Propagating
function a() {
  throw new Eror('error!');
}

function b() {
  a();
}

function c() {
  b();
}

try {
  c();
} catch (error) {
  console.log('Catched!');
}
console.log('Done!');

// 에러처리의 주체는 발생지점에서 근접한 함수일수도 있고, 최종적으로는 맨 처음 호출한 함수일 수도 있음
```

## 모듈 작성법

[JavaScript modules - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)

```jsx
let count = 0;
export function increase() {
  count++;
  console.log(count);
}

export function getCount() {
  return count;
}
```

```jsx
// import { increase as increase1 } from './counter.js';
// import { increase, getCount } from './counter.js';
import * as counter from './counter.js';

counter.increase();
counter.increase();
counter.increase();

const count = counter.getCount();
console.log(count);
```
