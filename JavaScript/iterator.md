# 이터러블, 제너레이터

## 이터러블이란?

- 이터레이션 → 반복, 순회
- 이터레이션 프로토콜
  [Iteration protocols - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)
  - Iterable Protocol
  - 반복/순회에 관련된 규격, 약속, 인터페이스
  - Object가 Iterable규격을 충족한다는 것은 아래와 같이 IterableIterator에 속해야 함
  ```jsx
  [Symbol.iterator]():IterableIterator<T>;
  ```

## 이터러블 살펴보기

```jsx
// Iterable : 순회가 가능하다는 것
// [Symbol.iterator](): IterableIterator<T>;
// 심볼 정의를 가진 객체나, 특정한 함수가 IterableIterator<T>를 리턴한다는 것은
// 순회 가능한 객체라는것을 의미함
// 순회가 가능하면 무엇이 가능한가?
// -> 반복문, 연산자들을 사용할 수 있다는 것
const array = [1, 2, 3];
console.log(array.values()); // Object [Array Iterator]{}
console.log(array.entries());
console.log(array.keys());

// iterator 사용방법
const iterator = array.values();
while (true) {
  const item = iterator.next();
  if (item.done) break;
  console.log(item.value);
}
for (let item of array.values()) {
  console.log(item);
}

const obj = { id: 123, name: 'tester' };
// object is not iterable
// for (const item of obj) {
//   console.log(item);
// }
for (const key in obj) {
  console.log(key);
}
```

## 이터러블 만들어보기

[Javascript Iterator](https://dev-momo.tistory.com/entry/Javascript-Iterator)

```jsx
// [Symbol.iterator](): IterableIterator<T>;
// 0부터 10이하까지 숫자의 2배를 순회하는 이터레이터(반복자) 만들기!
const makeIterable = (startValue, maxValue, callback) => {
  return {
    [Symbol.iterator]: () => {
      const max = maxValue;
      let num = startValue;
      return {
        next() {
          return { value: callback(num++), done: num > max };
        },
      };
    },
  };
};

console.clear();
for (const num of makeIterable(0, 20, (num) => num * 2)) {
  console.log(num);
}
```
