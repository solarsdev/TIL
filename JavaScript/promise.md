# 비동기

## 자바스크립트 실행 순서 (콜스택)

### 자바스크립트 런타임 환경

- 자바스크립트 엔진
  - 메모리 힙
  - 콜스택 (함수의 실행 순서를 기억함)
    ```jsx
    a()
    ---
    b()
    ---
    c()
    ```
    - Single Context Stack → Single Thread
    - 자바스크립트는 기본적으로 한번에 한가지만 (동기적으로) 실행됨
    ```jsx
    function a() {
      for (let i = 0; i < 1000000000; i++) {}
      return 1;
    }

    function b() {
      return a() + 1;
    }

    function c() {
      return b() + 1;
    }

    console.log('start here');
    const result = c();
    console.log(result);
    ```
    - 따라서 콜스택에 있는 특정 함수에서 시간이 오래 걸리는 작업을 수행하게 되면, 해당 여파가 호출한 함수뿐만 아니라 그 상위에 있는 함수에도 전파될 수 있음

## 콜백 비동기 사용해 보기

- 자바스크립트에서 모든 함수가 동기적으로 수행될까?
  - 그건 아님
  - Web APIs들은 기본적으로 비동기로 수행되도록 설계되어 있음
  - 즉, Multi Thread이기 때문에 동시 다발적으로 처리를 수행할 수 있음

```jsx
DOM APIs
setTimeout()
setInterval()
fetch()
event listener
```

- 예를 들면, 타임아웃 함수의 경우 전달한 콜백 함수를 일정 시간이 지난 후에 수행하는데, 기다리는 동안 처리가 일어나지 않는것이 아니고, 호출한 즉시 다음 구문이 수행됨 (병렬 처리)
- 자바스크립트 엔진은 콜스택에 있는 함수만 수행이 가능하기 때문에, 비동기 함수들은 수행되기 전까지는 `Task Queue`에 보존되어 있다가, 때가 되면 콜스택으로 올라감
- 콜스택과 Task Queue를 감시하는 객체가 바로 Event Loop

```jsx
function execute() {
  console.log('1');
  setTimeout(() => {
    console.log('2');
  }, 3000);
  console.log('3');
}

execute();
// 함수를 호출하면 1이 출력되고,
// setTimeout은 비동기이기 때문에 task queue에 저장하고 다음으로 이동
// 3을 출력하고,
// 3초 뒤에 task queue에서 call stack으로 콜백 함수를 가져옴
```

## 타임아웃 구현해보기

```jsx
// 주어진 seconds(초)가 지나면 callback함수를 호출함
// 단, seconds가 0보다 작으면 에러를 던지자!
function runInDelay(callback, seconds) {
  if (!callback) {
    throw new Error('callback함수를 전달해야 함');
  }
  if (seconds < 0) {
    throw new Error('0보다 작은 숫자를 입력할 수 없습니다.');
  }
  setTimeout(callback, seconds * 1000);
}

try {
  runInDelay(() => console.log('test'), 3);
} catch (error) {
  console.log(error);
}
```

## 프로미스 시작

[Promise - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

- 비동기처리를 좀 더 깔끔하게 구현하기 위한 방식

```jsx
then
catch
finally
```

- Promise 객체는 완료된 이벤트를 알려줌 (실패 성공 관계 없이)
- 비동기적으로 수행하고 결과를 반환함
- Promise의 상태 3가지
  - pending
  - fulfilled
  - rejected

```jsx
function runInDelay(seconds) {
  return new Promise((resolve, reject) => {
    if (seconds < 0) {
      reject(new Error('seconds가 0보다 작음'));
    }
    setTimeout(resolve, seconds * 1000);
  });
}

runInDelay(2)
  .then(
    // 필요한 작업 수행
    () => console.log('타이머 완료!')
  )
  .catch(
    // 에러를 처리
    console.error
  )
  .finally(
    // 최종적으로 반드시 실행
    () => console.log('끝!')
  );
```

- Promise 객체를 생성해서 반환해주는 함수를 만들면 해당 함수는 비동기적으로 수행할 수 있는 상태가 됨
- Promise 객체를 다루는 함수는 then catch finally를 사용할 수 있게 됨
- Promise 객체를 생성할때 생성자는 resolve, reject를 구현해야 함
  - resolve → 성공적으로 수행되었을때 수행되는 콜백함수
  - reject → 실패했을때 수행되는 콜백함수로 Error를 입력받음

## 프로미스 함수들

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  //return Promise.resolve(`🪴 => 🐔`);
  return Promise.reject(new Error('치킨을 가지고 올 수 없었다!'));
}

getChicken() //
  .catch((error) => {
    console.error(error);
    return '🐓';
  })
  .then(fetchEgg) //
  .then(fryEgg) //
  .then(console.log); //
```

## 프로미스 병렬 처리

```jsx
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}

// 바나나와 사과 가져오기
getBanana() //
  .then((banana) =>
    getApple() //
      .then((apple) => [banana, apple])
  )
  .then(console.log);
// Promise를 순차적으로 수행하면 총 시간이 늘어나므로 병렬수행이 필요
// Promise.all 병렬적으로 한번에 모든 Promise를 실행!
Promise.all([getBanana(), getApple()]) //
  .then((fruits) => console.log(`all: ${fruits}`));

// Promise.race 주어진 Promise들 중에 제일 빨리 수행된것이 승리
Promise.race([getBanana(), getApple()]) //
  .then((fruit) => console.log(`race: ${fruit}`));

// all 수행중 에러가 발생하면?
// catch를 수행해야 할 필요가 있음
Promise.all([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log(`all: ${fruits}`)) //
  .catch(console.error);

// Promise.all의 then절은 all에 전달한 모든 함수가 성공적으로 수행되었을때만 실행됨
// 일부만 성공해도 then을 순차적으로 실행하고 싶다면? -> allSettled
Promise.allSettled([getBanana(), getApple(), getOrange()]) //
  .then((fruits) => console.log(`all: ${fruits}`, fruits)) //
  .catch(console.log);
// allSettled는 각각의 결과값을 객체 배열의 형태로 전달해주고, 상태와 값을 따로 전달해줌
// fulfilled -> 성공한 구문
// rejected -> 실패한 구문
```

## 비동기 코드를 동기적으로 async, await

```jsx
function getBanana() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍌');
    }, 1000);
  });
}

function getApple() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('🍎');
    }, 3000);
  });
}

function getOrange() {
  return Promise.reject(new Error('no orange'));
}

// 바나나와 사과 가져오기
async function fetchFruits() {
  const banana = await getBanana();
  const apple = await getApple();
  return [banana, apple];
}

fetchFruits().then((fruits) => console.log(fruits));
```

## async 퀴즈

```jsx
function fetchEgg(chicken) {
  return Promise.resolve(`${chicken} => 🥚`);
}

function fryEgg(egg) {
  return Promise.resolve(`${egg} => 🍳`);
}

function getChicken() {
  return Promise.reject(new Error('can not find 🐓'));
}

async function getFriedEgg() {
  let chicken;
  try {
    chicken = await getChicken();
  } catch (error) {
    chicken = '🐔';
  }
  const egg = await fetchEgg(chicken);
  const friedEgg = await fryEgg(egg);
  return friedEgg;
}

getFriedEgg().then(console.log);
```

## JSON에 대해서

```jsx
// JSON: JavaScript Object Notation
// 서버와 클라이언트(브라우저, 모바일) 간의 HTTP 통신을 위한
// 오브젝트 형태의 텍스트 포맷
// stringify(object) -> JSON
// parse(JSON) -> object
const lee = {
  name: 'lee',
  age: 13,
  eat: () => {
    console.log('eat');
  },
};

// 직렬화 Serializing: 객체를 문자열로 변환
const json = JSON.stringify(lee);
// stringify를 통해 만들어지는 text내에는 property만 변환되어 저장됨
// method의 경우에는 변환되지 않음
console.log(lee);
console.log(json);

// 역직렬화 Deserializing: 문자열 데이터를 자바스크립트 객체로 변환
const obj = JSON.parse(json);
console.log(obj);
```

## 네트워크 통신 사용하기

[Fetch API - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

[](https://www.7timer.info/bin/astro.php?lon=113.2&lat=23.1&ac=0&unit=metric&output=json&tzshift=0)

```jsx
fetch(
  'http://www.7timer.info/bin/api.pl?lon=113.17&lat=23.09&product=astro&output=json'
) //
  .then((response) =>
    // json() 함수는 데이터를 json으로 받아오는 역할을 수행하는 것이 아님
    // 본문에 있는 json을 JavaScript에서 사용 가능한 객체로 변환하는 역할
    // Promise를 반환하기 때문에 비동기 처리 then, aync await등이 필요
    response.json()
  )
  .then((data) => console.log(data.dataseries));
```
