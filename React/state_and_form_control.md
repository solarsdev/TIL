### State

- React의 함수형 hook에 의해서 사용할수 있는 State로는 useState()가 있음
- TypeScript와 함께 사용할 때 useState의 경우 기본값을 주게되면 기본값의 논리형에 근거해서 TypeScript가 추론한 논리형이 변수에 부여되고, setter또한 자동으로 형태가 지정됨
  - 이는 합리적인 추론으로, state를 작성하는 시점에 부여되는 형태가 변경되는 경우는 대부분 없음
  - 하지만 복수형태의 변수또한 존재할 수는 있는데, 이를 작성시점에 부여할 수 있음
    ```jsx
    const [value, setValue] = (useState < number) | (string > 1);
    setValue('hello'); // OK
    setValue(2); // OK
    setValue(true); // NOT OK
    ```

### Form

- 폼을 이용하는 기본 소스코드를 일단 첨부

```jsx
import { useState } from 'react';

const App = () => {
  const [value, setValue] = useState('');
  const onChange = (event) => {};
  return (
    <div>
      <form>
        <input type='text' placeholder='username' value={value} onChange={onChange} />
        <button>Login</button>
      </form>
    </div>
  );
};

export default App;
```

- TypeScript에서는 onChange() 함수의 event를 매개변수로 하는 부분에서 에러가 발생함

  - JavaScript에서 event는 any타입으로 설정되어 있기 때문
  - TypeScript를 위해서 event에서 어떤 타입을 지정해야 할까?
    <aside>
    💡 React.FormEvent를 event에 지정해주면, 에러가 해결됨

    </aside>

- React Forms and Events에서 TypeScript를 사용시 참고
  [Forms and Events | React TypeScript Cheatsheets](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forms_and_events/)
- **☕️ SyntheticEvent**에 대해서
  ```jsx
  const onClick = (event: React.FormEvent<HTMLButtonElement>) => {};
  ```
  [SyntheticEvent - React](https://reactjs.org/docs/events.html)
  - 위와 같이 React 기반으로 폼이벤트를 지정해서 해당 폼 안에 있는 엘리먼트를 직접 지정하거나, 마우스 이벤트등과 같이 별도의 React만의 이벤트 정의를 이용하게 되는데, 타입스크립트는 이벤트의 타입을 React 전용 타입으로 지정할 뿐임
  - 따라서 이러한 방식은 React에 의존한 코딩방식이 되며, 다른 라이브러리(예를 들어 Vue나 Angular)에서는 다른 이벤트 정의 방식을 사용할 수 있음
- 이처럼 React와 TypeScript를 동시에 사용하면서, 서로 보완하는 문법은 배워야 하지만 크게 바뀌지 않고 여러번 사용하다보면 패턴처럼 몸에 익을수 있음
- 간단한 유저이름을 입력받고, 로그인 버튼을 누르면 콘솔에 출력되는 어플리케이션

  ```jsx
  import React, { useState } from 'react';

  const App = () => {
    const [value, setValue] = useState('');
    const onChange = (event: React.FormEvent<HTMLInputElement>) => {
      setValue(event.currentTarget.value);
    };
    const onSubmit = (event: React.FormEvent<HTMLFormElement>) => {
      event.preventDefault();
      console.log(`hello ${value}`);
    };
    return (
      <div>
        <form onSubmit={onSubmit}>
          <input type='text' placeholder='username' value={value} onChange={onChange} />
          <button>Login</button>
        </form>
      </div>
    );
  };

  export default App;
  ```

    <aside>
    💡 TypeScript를 이용한 React.FormEvent의 활용으로 런타임이 아닌 컴파일타임에 에러를 발견할 수 있음
    
    </aside>
