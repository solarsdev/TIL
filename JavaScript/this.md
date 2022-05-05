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
