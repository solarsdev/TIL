# 배열

## 자료구조, 배열이란?

### 복습 (객체의 속성)

- 객체의 경우에는 비슷한 속성과 행동을 한데 묶에 만들어두는 템플릿을 말함
- 클래스의 생성자함수를 이용해서 템플릿을 잘 만들어 두면, 인스턴스를 생성하는데 활용할 수 있음

### 자료구조

- 객체를 만든 뒤에는 해당 객체들끼리 모아두는 방이 있을 수 있음 (배열)
- 서버에 입장하는 스레드 단위로 순서를 매기고, 순서에 따라서 처리를 진행할수도 있음 (큐)
- 즉, 객체의 집합체를 나타낼수 있는것이 자료구조

### 배열

- 순서표대로 인덱스를 나눠주고 처리해야 할 경우 활용 가능
- 메모리상에서는 순서대로 하나씩 연결되어 있고, 인덱스로 표현할 수 있음
- 인덱스는 0부터 시작함
- 자바스크립트에서는 타입이 정해져 있지 않기 때문에, 배열 안에 다양한 타입의 데이터를 담을 수 있지만 일반적으로 선호되지는 않음

## 배열 만들기

[Array - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

- 원래 자바스크립트에서는 Array타입 하나만 있었으나, 최근에는 데이터 타입이 지정된 배열도 존재하고 해당 배열에는 같은 데이터 타입만 담아야 함

```jsx
// 배열 생성 방법
let array = new Array(3);
console.log(array);

array = new Array(1, 2, 3); // 생성자함수를 이용
console.log(array);

array = Array.of(1, 2, 3, 4, 5); // static 함수
console.log(array);

const anotherArray = [1, 2, 3, 4];
console.log(anotherArray);

// iterable - 순회가 가능한 것들 (문자열, 배열)
array = Array.from(anotherArray); // static 함수
console.log(array);

array = Array.from('text'); // static 함수
console.log(array);

// 일반적으로 배열은 동일한 메모리 크기를 가지며, 연속적으로 이어져 있어야함
// 하지만 자바스크립트에서의 배열은 연속적으로 이어져 있지 않을 수 있음
// Object와 비슷함
// 자바스크립트에서의 배열은 일반적인 배열의 동작을 흉내낸 특수한 객체 (object)
// 이걸 보완하기 위해서 타입이 정해져 있는 타입 배열이 있음 (Typed Collections)
array = Array.from({
  0: '안',
  1: '녕',
  length: 2,
});
console.log(array);
```

## 배열을 다루면서 하지 말아야 할 것

```jsx
const fruits = ['🍌', '🍎', '🍇', '🍊'];

// 배열 아이템을 참조하는 방법
console.log(fruits[0]);
console.log(fruits[1]);
console.log(fruits[2]);
console.log(fruits[3]);
console.log(fruits.length);

for (let i = 0; i < fruits.length; i++) {
  const element = fruits[i];
  console.log(element);
}

// 추가, 삭제 - 좋지 않은 방식
// const fruits = ['🍌', '🍎', '🍇', '🍊'];
//fruits[4] = 'ichigo';
fruits[fruits.length - 1] = 'ichigo';
// 인덱스로 접근하는 것은 좋지 못함
delete fruits[1];
// 키워드로 삭제하면 배열 도중에 공백이 생기기 때문에 좋지 못함
```

## 배열 함수

```jsx
// 배열의 함수들
// 배열 자체를 변경하는지, 새로운 배열을 반환하는지
const fruits = ['🍌', '🍎', '🍇', '🍊'];

// 특정한 오브젝트가 배열인지 체크
console.log(Array.isArray(fruits));
console.log(Array.isArray({}));

// 특정한 아이템의 위치를 찾을때
console.log(fruits.indexOf('🍎'));

// 배열안에 특정한 아이템이 있는지
console.log(fruits.includes('🍎'));

// 추가 1. 제일 뒤에 추가
console.log(fruits.push('🍑')); // 배열 자체를 수정 (업데이트)
console.log(fruits);

// 추가 2. 제일 앞에 추가
console.log(fruits.unshift('🍉')); // 배열 자체를 수정 (업데이트)
console.log(fruits);

// 제거 1. 제일 뒤를 제거
console.log(fruits.pop()); // 배열 자체를 수정 (업데이트)
console.log(fruits);

// 제거 2. 제일 앞을 제거
console.log(fruits.shift()); // 배열 자체를 수정 (업데이트)
console.log(fruits);

// 중간을 제거
console.log(fruits.splice(1, 1));
console.log(fruits);

// 중간에 추가
console.log(fruits.splice(1, 0, '🍎', '🍓'));
console.log(fruits);

// 잘라진 새로운 배열을 만듬
let newArr = fruits.slice(0, 2);
console.log(newArr);
console.log(fruits);

// slice 아무것도 전달하지 않을때
newArr = fruits.slice();
console.log(newArr);

// 여러개의 배열을 붙여줌
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = arr1.concat(arr2);
console.log(arr1);
console.log(arr2);
console.log(arr3);

// 배열의 순서를 거꾸로 변경
const arr4 = arr3.reverse();
console.log(arr4);

console.clear();
// 중첩 배열을 하나의 배열로 쫙 펴기
let arr = [
  [1, 2, 3],
  [4, [5, 6]],
];
console.log(arr);
console.log(arr.flat(2));
arr = arr.flat(3);

// 특정한 값으로 배열을 채우기
arr.fill(0);
console.log(arr);

arr.fill('s', 1, 4);
console.log(arr);

// 배열을 문자열로 합하기
let text = arr.join(' | ');
console.log(text);
```

## 메모리 중요한 포인트!

```jsx
const pizza = { name: '🍕', price: 2 };
const ramen = { name: '🍜', price: 3 };
const sushi = { name: '🍣', price: 1 };
const store1 = [pizza, ramen];
const store2 = Array.from(store1); // shallow copy
console.log('store1', store1);
console.log('store2', store2);

store2.push(sushi);
console.log('store1', store1);
console.log('store2', store2);

pizza.price = 4;
console.log('store1', store1);
console.log('store2', store2);

// Array.from을 이용해서 복사를 하면 배열 내 값들이 복사됨
// 값이 객체의 주소일 경우 주소만 복사됨
// 다른 배열의 객체 실은 동일한 것일 수 있음 (얕은 복사)
// 자바스크립트에서는 복사할때 항상 얕은 복사가 이루어짐!
// Array.from, concat, slice, spread(...), Object.assign
```

## 퀴즈1

```jsx
// 퀴즈1: 주어진 배열 안의 딸기 아이템을 키위로 교체하는 함수를 만들기
// 단, 주어진 배열을 수정하지 않도록!
// input: ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🥝', '🍇', '🥝' ]
function turnStrawberryIntoKiwiOnArray(original = []) {
  return original.map((item) => {
    if (item === '🍓') {
      return '🥝';
    }
    return item;
  });
}
console.log(turnStrawberryIntoKiwiOnArray(['🍌', '🍓', '🍇', '🍓']));

// 퀴즈2:
// 배열과 특정한 요소를 전달받아,
// 배열안에 그 요소가 몇개나 있는지 카운트 하는 함수 만들기
// input: [ '🍌', '🥝', '🍇', '🥝' ], '🥝'
// output: 2
function countItemOnArray(target = [], item) {
  return target.filter((itemOnTarget) => itemOnTarget === item).length;
}
console.log(countItemOnArray(['🍌', '🥝', '🍇', '🥝'], '🥝'));

// 퀴즈3: 배열1, 배열2 두개의 배열을 전달받아,
// 배열1 아이템중 배열2에 존재하는 아이템만 담고 있는 배열 반환
// input: ['🍌', '🥝', '🍇'],  ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🍇' ]
function filterItemIsOnTheAnotherArray(target = [], another = []) {
  return target.filter((targetItem) => another.includes(targetItem));
}
console.log(
  filterItemIsOnTheAnotherArray(['🍌', '🥝', '🍇'], ['🍌', '🍓', '🍇', '🍓'])
);
```

## 고차함수란?

### 일급객체 복습, 함수형 프로그래밍이란?

- 일급객체 (일급함수)
  - 일반 객체처럼 모든 연산이 가능한 것
    - 함수의 매개변수로 전달
    - 함수의 반환값
    - 할당 명령문
    - 동일 비교 대상
- 고차함수
  - 인자로 함수를 받거나 (콜백함수)
  - 함수를 반환하는 함수
- 함수형 프로그래밍
  - 절차지향식 → 코드 순서(위에서 아래)에 따라서 제어문을 통해 결과를 도출
  - 함수형 프로그래밍은 작은 작업단위들을 함수로 전부 작성해두고, 함수의 연쇄 호출에 의해서 결과를 도출하는 방식
  - 작업의 작은 단위를 순수함수로 만들어두는 것이 중요
    - 순수함수란?
    - 함수 내에서 매개변수를 수정한다거나 하는 부작용(사이드이펙트)없이 항상 동일한 결과가 예측(불변성)되는 함수
  - 함수형 프로그래밍을 하게 되면
    - 에러는 감소
    - 가독성이 증가
    - 데이터를 변경하지 않음
    - 변수 사용 하지 않음
    - 조건문과 반복문과 같은 제어문을 사용하지 않음

### 배열에서도 함수형 프로그래밍이 가능하다고?

```jsx
const fruits = ['🍌', '🍓', '🍇', '🍓'];
// for (let i = 0; i < fruits.length; i++) {
//   console.log(fruits[i]);
// }
// 배열을 순회하며 원하는 것을 할때 forEach
// fruits.forEach((value, index, array) => {
//   console.log(value);
//   console.log(index);
//   console.log(array);
//   console.log('-----');
// });
fruits.forEach((item) => console.log(item));

// 조건에 맞는(콜백함수) 아이템을 찾을때
// find: 제일 먼저 조건에 맞는 아이템을 반환
const item1 = { name: '🥛', price: 2 };
const item2 = { name: '🍪', price: 3 };
const item3 = { name: '🍙', price: 1 };
const products = [item1, item2, item3, item2];
let result = products.find((value) => value.name === '🍪');
console.log(result);

// findIndex: 제일 먼저 조건에 맞는 인덱스를 반환
result = products.findIndex((value) => value.name === '🍪');
console.log(result);

// 배열의 아이템들이 부분적으로 조건(콜백함수)에 맞는지 확인
result = products.some((item) => item.name === '🍪');
console.log(result);

// 배열의 아이템들이 전부 조건(콜백함수)에 맞는지 확인
result = products.every((item) => item.name === '🍪');
console.log(result);

// 조건에 맞는 모든 아이템들을 새로운 배열로 !
result = products.filter((item) => item.name === '🍪');
console.log(result);

console.clear();

// Map 배열의 아이템들을 각각 다른 아이템으로 매핑할 수 있는, 변환해서 새로운 배열 생성
const nums = [1, 2, 3, 4, 5];
result = nums.map((item) => item * 2);
console.log(result);
result = nums.map((item) => {
  if (item % 2 === 0) {
    return item * 2;
  }
  return item;
});
console.log(result);

// flatMap: 중첩된 배열을 펴줌
result = nums.map((item) => [1, 2]);
console.log(result);
result = nums.flatMap((item) => [1, 2]);
console.log(result);

result = ['hello', 'world!'].map((text) => text.split(''));
console.log(result);
result = ['hello', 'world!'].flatMap((text) => text.split(''));
console.log(result);

// sort: 배열의 아이템들을 정렬
// 문자열 형태의 오른차순으로 요소를 정렬하고, 기존의 배열을 변경
const texts = ['hi', 'abc'];
texts.sort();
console.log(texts);

const numbers = [0, 5, 4, 2, 1, 10];
numbers.sort();
console.log(numbers);
// 문자열순서이기 때문에, 1 다음 10
// 전달한 콜백함수의 결과값이 음수라면 앞으로 정렬 (오름차순)
// 양수라면 내림차순
numbers.sort((a, b) => a - b);
console.log(numbers);

// reduce 배열의 요소들을 접어서 값을 하나로 만들어나감
result = [1, 2, 3, 4, 5].reduce((sum, value) => (sum += value), 0);
console.log(result);
```

## 고차함수 퀴즈 2

```jsx
// 퀴즈 4
// 5이상(보다 큰)의 숫자들의 평균
const nums = [3, 16, 5, 25, 4, 34, 21];

console.log(
  nums
    .filter((num) => num > 5)
    .reduce((avg, num, _, arr) => (avg + num) / arr.length)
);
```
