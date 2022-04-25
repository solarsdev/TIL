# Map과 Set

## Set과 함수들

[Set - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

```jsx
// Set
const set = new Set([1, 2, 3]);
console.log(set);

// 갯수
console.log(set.size);

// 존재하는지 확인
console.log(set.has(2));
console.log(set.has(4));

// 순회
set.forEach((val) => console.log(val));
for (const val of set.values()) {
  console.log(val);
}

// 추가
set.add(4);
console.log(set);
set.add(4);
console.log(set);
// 중복이 안됨

// 삭제
set.delete(4);
console.log(set);

// 전부 다 삭제
set.clear();
console.log(set.size);
console.log(set);

// 오브젝트 셋
const obj1 = { name: 'lee', age: 23 };
const obj2 = { name: 'kim', age: 21 };
const objs = new Set([obj1, obj2]);
console.log(objs);

// 퀴즈
obj1.age = 13;
objs.add(obj1);
console.log(objs);

// 퀴즈
const obj3 = { name: 'kim', age: 21 };
objs.add(obj3);
console.log(objs);
```

## Map과 함수들

[Map - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

```jsx
const map = new Map([
  ['key1', '🍎'],
  ['key2', '🍌'],
]);
console.log(map);

// 사이즈
console.log(map.size);

// 존재확인
console.log(map.has('key1'));
console.log(map.has('key3'));

// 순회
map.forEach((value, key) => {
  console.log(key, value);
});
console.log(map.keys());
console.log(map.values());
console.log(map.entries());

// 찾기
console.log(map.get('key1'));
console.log(map.get('key2'));
console.log(map.get('key3'));

// 추가
map.set('key3', '🥝');
console.log(map);

// 삭제
map.delete('key3');
console.log(map);

// 전부삭제
map.clear();
console.log(map);

// 오브젝트와의 차이점??
const key = { name: 'milk', price: 10 };
const milk = { name: 'milk', price: 10, description: '맛있는 우유' };
const obj = {
  [key]: milk,
};
console.log(obj);

const map2 = new Map([[key, milk]]);
console.log(map2);
```

## Set 퀴즈

```jsx
// 주어진 배열에서 중복을 제거 하라
const fruits = ['🍌', '🍎', '🍇', '🍌', '🍎', '🍑'];
//  ['🍌', '🍎', '🍇', '🍑']
console.log([...new Set(fruits)]);

// 주어진 두 세트의 공통된 아이템만 담고 있는 세트 만들어라
const set1 = new Set([1, 2, 3, 4, 5]);
const set2 = new Set([1, 2, 3]);
console.log(new Set([...set1].filter((item) => set2.has(item))));
```

## Symbol

[Symbol - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

```jsx
// Symbol 심볼
// 유일한 키를 생성할 수 있음
const map = new Map();
// const key1 = 'key';
// const key2 = 'key';
// map.set(key1, 'Hello');
// console.log(map.get(key2));
// console.log(key1 === key2);

const key1 = Symbol('key');
const key2 = Symbol('key');
map.set(key1, 'Hello');
console.log(map.get(key2));
console.log(key1 === key2);

// 동일한 이름으로 하나의 키를 사용하고 싶다면, Symbol.for
// 전역 심벌 레지스트리 (Global Symbol Registry)
const k1 = Symbol.for('key');
const k2 = Symbol.for('key');
console.log(k1 === k2);

console.log(Symbol.keyFor(k1));
console.log(Symbol.keyFor(key1));

// 심벌을 언제 사용?
// Map에서 key를 사용할때 문자열 대신에 symbol을 사용하면 유니크한 키를 만들어낼 수 있음
// object에서도 symbol 사용 가능
const obj = { [k1]: 'Hello', [Symbol('key')]: 1 };
console.log(obj);
console.log(obj[Symbol('key')]);
```
