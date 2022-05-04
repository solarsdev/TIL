# 클로저

## 클로저 개념 정리

- 클로저 closures → 폐쇄, 닫히는 느낌
  - 클로저는 함수와 렉시컬 환경의 조합
  - 클로저는 내부함수에서 외부 함수에 접근할 수 있는 권한을 주는 것
    - 내부 함수에서 외부에 있는 스코프에 접근이 가능
    - 렉시컬 환경을 통해 기본적으로 내부 블럭에서 외부 블럭에 접근이 가능

```jsx
function outer() {
  const x = 0;
  function inner() {
    x;
  }
  inner();
}
outer();
```

- 상기 코드가 실행될때 context stack을 확인해보면
  1. 전역 스코프 렉시컬 환경
  2. outer 스코프 렉시컬 환경
     1. x 에 대한 환경 레코드 작성
  3. inner 스코프 렉시컬 환경

```jsx
function outer() {
  const x = 0;
  function inner() {
    x;
  }
  return inner;
}
const inner = outer();
inner();
```

1. 전역 스코프 렉시컬 환경
2. outer 스코프 렉시컬 환경
3. inner 스코프 렉시컬 환경

- 실행 컨텍스트 스택에서는 코드가 실행되면서 outer와 inner는 컨텍스트 스택에서 제거가 됨
- 변수에는 outer를 참조하기 때문에 메모리에는 남아 있으며, 렉시컬 환경의 참조값을 통해 outer함수의 환경 레코드에 접근 가능함

## 클로저 코드로 확인

```jsx
const text = 'hello';
function func() {
  console.log(text);
}
func();

// 중첩된 함수에서 내부 함수에서 외부 함수로의 데이터에 접근이 가능
// 내부 함수와 외부 함수가 같이 엮여 있어서 하나의 그릅 (닫혀있는) 으로 구성되어 있는 것

function outer() {
  const x = 0;
  function inner() {
    console.log(`inside inner: ${x}`);
  }
  inner();
}
//outer();
const func1 = outer;
func1();
func1();
```

## 클로저 활용 예제들

[Closures - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

```jsx
// 내부 정보를 은닉하고, 공개 함수(public, 외부 함수)를 통한 데이터 조작을 위해
// 캡슐화와 정보은닉
// 클래스 private 필드 또는 메소드를 사용하는 효과와 동일
function makeCounter() {
  let count = 0;
  function increase() {
    count++;
    console.log(count);
  }
  return increase;
}

const increase = makeCounter();
increase();
increase();
increase();

class Counter {
  #count = 0;
  increase() {
    this.#count++;
    console.log(this.#count);
  }
}

const counter = new Counter();
counter.increase();
counter.increase();
counter.increase();
```
