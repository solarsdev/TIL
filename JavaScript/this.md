# This의 비밀

## This 바인딩이란?

- 자바스크립트에서 this란 문맥에 따라 가리키는 것이 달라짐
- 일반적으로는 객체나 클래스 안에서 this를 활용하게 되는데, 앞으로 만들어질 인스턴스나 객체 자신을 가리키는 용도 활용됨 → this 바인딩
- 자바나 C#에서 사용되는 this는 코드상에서 정적으로 결정됨
- 자바스크립트나 타입스크립트에서는 런타임상에서 this 바인딩이 결정됨

## 문맥에서의 this

```jsx
/**
 * 글로벌 컨텍스트의 this
 *  - 브라우저:
 *  - 노드:
 */
const x = 0;
module.exports.x = x;
// commonJS module
console.log(this);
console.log(globalThis); // 전역 객체
//globalThis.setTimeout();
//setTimeout();

// 브라우저
// this -> Window
// globalThis -> Window

console.clear();

/**
 * 함수 내부에서의 this
 * 엄격모드에서는 undefined
 * 느슨한모드에서는 globalThis
 */
function fun() {
  console.log(this);
}
fun();
// 노드에서는 함수 내부에서 this는 globalThis
// use strict를 통해서 엄격모드에서는 함수 내부에서 this를 사용할 수 없음
// 브라우저에서는 Window 객체
// 클래스 내부에서는 인스턴스를 가리킴

/**
 * 생성자 함수 또는 클래스에서의 this
 * 앞으로 생성될 인스턴스 자체
 */
function Cat(name) {
  this.name = name;
  this.printName = function () {
    console.log(this.name);
  };
}
const cat1 = new Cat('냐옹');
const cat2 = new Cat('니야옹');
cat1.printName();
cat2.printName();
```

## 동적 바인딩

```jsx
// this 바인딩
// 자바, C#, C++ 대부분의 객체지향 프로그래밍 언어에서는
// this는 항상 자신의 인스턴스 자체를 가리킴!
// 정적으로 인스턴스가 만들어지는 시점에 this가 결정됨
// 그러나 자바스크립트에서는 누가 호출하느냐에 따라서 this가 달라짐
// 즉, this는 호출하는 사람(caller)에 의해서 동적으로 결정됨
function Cat(name) {
  this.name = name;
  this.printName = function () {
    console.log(`고양이의 이름을 출력한다옹: ${this.name}`);
  };
}

function Dog(name) {
  this.name = name;
  this.printName = function () {
    console.log(`강아지의 이름을 출력한다옹: ${this.name}`);
  };
}

// 생성자로 인스턴스를 생성하면 this는 그때 해당 인스턴스의 변수와 함수를 연결해줌
// -> 동적 바인딩
const cat = new Cat('냐옹');
const dog = new Dog('멍멍');
cat.printName();
dog.printName();

dog.printName = cat.printName;
dog.printName();
cat.printName();

function printOnMonitor(printName) {
  console.log('모니터를 준비하고, 전달된 콜백을 실행');
  printName();
}

printOnMonitor(cat.printName);
// undefined
// 정적 언어에서는 this가 반영이 되어 있을것이기 때문에
// 해당 객체의 printName을 호출했을것
```

## 정적 바인딩

```jsx
function Cat(name) {
  this.name = name;
  // 2. arrow 화살표 함수를 사용: arrow 함수는 렉시컬 환경에서의 this를 기억하고 있음
  // 화살표 함수 밖에서 제일 근접한 스코프의 this를 가리킴
  this.printName = () => {
    console.log(`고양이의 이름을 출력한다옹: ${this.name}`);
  };
  // 1. bind 함수를 이용해서 수동으로 바인딩
  // this.printName = this.printName.bind(this);
}

function Dog(name) {
  this.name = name;
  this.printName = function () {
    console.log(`강아지의 이름을 출력한다옹: ${this.name}`);
  };
}

// 생성자로 인스턴스를 생성하면 this는 그때 해당 인스턴스의 변수와 함수를 연결해줌
// -> 동적 바인딩
const cat = new Cat('냐옹');
const dog = new Dog('멍멍');
cat.printName();
dog.printName();

dog.printName = cat.printName;
dog.printName();
cat.printName();

function printOnMonitor(printName) {
  console.log('모니터를 준비하고, 전달된 콜백을 실행');
  printName();
}

printOnMonitor(cat.printName);
// undefined
// 정적 언어에서는 this가 반영이 되어 있을것이기 때문에
// 해당 객체의 printName을 호출했을것
```

## 화살표 함수 정리

```jsx
// 자바스크립트의 함수는 만능 슈퍼맨
// 함수처럼 사용도 가능하지만, 생성자 함수로도 사용 가능 (클래스)
// 옛날 자바스크립트는 클래스가 없었기 때문에 허용했었음
// 하지만, 이걸 위해서 불필요한 프로토타입 객체가 필요함 (데이터의 집합 -> 사용하지 않는다면 불필요)
const dog = {
  name: 'Dog',
  play: () => {
    // 좋지 못한 예
    // 객체의 키로 함수를 만들게 되면 함수의 프로토타입형으로 만들어 지게 됨
    console.log('논다멍');
  },
};
dog.play();
// const obj = new dog.play(); // ...
// console.log(obj);

// ES6
const cat = {
  name: 'cat',
  play() {
    // 객체의 메소드 (오브젝트에 속한 함수)
    console.log('냥');
  },
};
cat.play();
// const obj1 = new cat.play();
// cat.play는 생성자가 아님
// 즉, 객체의 메소드로 함수를 생성하면 객체의 생성자로는 사용할 수 없음

/**
 * 화살표 함수의 특징
 * 1. 문법이 깔끔
 * 2. 생성자 함수로 사용이 불가능 (무거운 프로토타입이 없음)
 * 3. 함수 자체 arguments가 없음
 * 4. this에 대한 바인딩이 정적으로 결정됨
 *    - 함수를 감싸고 있는, 바로 상위 스코프의 this에 정적으로 바인딩
 */

function sum(a, b) {
  console.log(arguments);
}
sum(1, 2);

const add = (a, b) => {
  console.log(arguments); // arrow함수 외부의 arguments를 참조함
};
add(1, 2);

const printArrow = () => {
  console.log(this);
};
printArrow();
cat.printName = printArrow;
cat.printName();
```
