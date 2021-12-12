## Introduction (React Master Class)

- 타입스크립트는 자바스크립트를 기반으로 한 프로그래밍 언어
- 기반으로 했기 때문에 문법등이 전혀 다른것은 아니며, 거의 비슷하고 몇몇 부분만 추가되었을 뿐임
- 타입스크립트는 strongly-typed한 언어이며 strongly-typed? 프로그래밍 언어가 동작하기 전에 타입을 확인함 (컴파일)

```jsx
// JavaScript 방식
const plus = (a, b) => a + b;
plus(2, 2); // 4
plus(2, 'hi'); // '2hi'

// 개발자의 의도가 숫자를 더하는 함수를 만들고 싶었다고 하더라도,
// JavaScript는 타입체크를 하지 않기 때문에 두번째 결과가 발생할 수 있음
```

<aside>
💡 개발자의 의도를 코드에 명시해서, JavaScript가 의도에 맞지 않는 매개변수를 입력받았을 경우에는 알림을 주었으면 좋겠다는 의도에서 탄생한 TypeScript

</aside>

```jsx
// 두번째 예제
const user = {
  firstName: 'Angela',
  lastName: 'Davis',
  role: 'Professor',
};
console.log(user.name); // undefined

// name이 존재하지 않기 때문에 JavaScript는 undefined를 출력하지만,
// 개발자에게는 정의되지 않은 오브젝트 내의 속성을 호출할경우 에러를 출력해주는것이 도움이 됨
```

<aside>
💡 의도와 다른 방향(실수)으로 코드를 작성했을때 해당 부분을 알려주는 특성이 있는 언어가 개발자 친화적이라고 할 수 있음

</aside>

- 런타임에서 에러가 발견되는것은 때에 따라서 치명적일 수 있음
- 타입스크립트는 그러한 에러를 프로그램이 가동되기 전에 미리 알려주는 기능을 탑재하고 있음
- TypeScript Playground
  [TS Playground - An online editor for exploring TypeScript and JavaScript](https://www.typescriptlang.org/play)
  ```jsx
  // TypeScript 방식
  const plus = (a: number, b: number) => a + b;
  plus(2, 2); // 4
  plus(2, 'hi'); // error
  ```
    <aside>
    💡 매개변수에 타입을 미리 정의함으로써 코드상에서의 잘못을 지적할 수 있음
    
    </aside>
