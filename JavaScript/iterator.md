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

## 제너레이터란?

- 제너레이터도 이터레이션 프로토콜을 준수하지만, 간단한 방식으로 이터레이터를 만들 수 있음
- function 키워드 다음 _을 부착하면 제너레이터가 됨 `function_`

```jsx
// const makeIterable = (startValue, maxValue, callback) => {
//   return {
//     [Symbol.iterator]: () => {
//       const max = maxValue;
//       let num = startValue;
//       return {
//         next() {
//           return { value: callback(num++), done: num > max };
//         },
//       };
//     },
//   };
// };

// 제너레이터를 만들게 되면 next를 호출해야 yield 다음 구문이 수행됨
// -> 제너레이터 내부의 yield를 만나게 되면, value, done이 반환됨과 동시에 next() 호출 전까지 코드대기
function* multipleGenerator() {
  for (let i = 0; i < 10; i++) {
    console.log(i);
    yield i ** 2;
  }
}

const multiple = multipleGenerator();
let next = multiple.next();
console.log(next.value, next.done);
multiple.return(); // return을 호출하면 iterator가 즉시 종료되고 done은 true가 됨
//multiple.throw('Error!'); // 에러 발생시키고 종료시킴 try-catch로 에러처리 가능
next = multiple.next();
console.log(next.value, next.done);
```

## 전개구문 연산자

```jsx
// Spread Operator, 전개구문
// 모든 Iterable은 Spread될 수 있음
// 순회가 가능한 모든 것들은 펼쳐질수 있음
// function (...iterable)
// [...iterable]
// {...iterable}
// ES 2018
function add(a, b, c) {
  return a + b + c;
}

const nums = [1, 2, 3];
console.log(add(...nums));

// Rest parameters
function sum(...nums) {
  return nums.reduce((result, num) => result + num);
}
console.log(sum(1, 2, 3, 4, 5, 6));

// Array Concat
const fruits1 = ['🍏', '🥝'];
const fruits2 = ['🍓', '🍌'];
let arr = fruits1.concat(fruits2);
console.log(arr);
arr = [...fruits1, '🍕', ...fruits2];
console.log(arr);

// Object
const lee = { name: 'lee', age: 22 };
const updated = {
  ...lee,
  job: 'engineer',
};
console.log(lee);
console.log(updated);
```

## 구조분해 할당

[Destructuring assignment - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

```jsx
// 구조 분해 할당 Destructuring Assignment
// 데이터 뭉치(그룹화)를 쉽게 만들 수 있다
const fruits = ['🍏', '🥝', '🍓', '🍌'];
const [first, second, ...others] = fruits;
console.log(fruits[0]);
console.log(first);
console.log(second);
console.log(others);

const point = [1, 2];
const [y, x, z = 0] = point;
console.log(x);
console.log(y);
console.log(z);

function createEmoji() {
  return ['apple', '🍎'];
}
const [title, emoji] = createEmoji();
console.log(title, emoji);

// Object Destructuring
const lee = { name: 'lee', age: 22 };
function display({ name, age }) {
  console.log('이름', name);
  console.log('나이', age);
}
display(lee);

const { name, age } = lee;
console.log(name);
```
