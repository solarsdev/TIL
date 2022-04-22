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
