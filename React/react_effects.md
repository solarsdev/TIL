## React Effects

- 기존방식으로 state를 이용해서 값을 정의하고, 새로운 값이 지정되면 전체 컴포넌트가 새로고침됨

```jsx
const App = () => {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prevValue) => prev + 1);

  // 여기에 작성된 코드는 state가 변경되면 계속해서 재실행됨
  console.log('rendered');

  // oops, this is not necessary to run again
  console.log('call an API');

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onClick}>click me</button>
    </div>
  );
};
```

- 만약 처음 컴포넌트가 로딩될때 처음에만 렌더링 하고, 그 이후에는 새로고침하고 싶지 않은 코드가 있을 경우에는 어떻게 해야 할까? (call an API 부분)
- 이런경우에 사용하는것이 바로 useEffect() 기능임

```jsx
const App = () => {
  const [counter, setValue] = useState(0);
  const onClick = () => setValue((prevValue) => prev + 1);

  // 여기에 작성된 코드는 state가 변경되면 계속해서 재실행됨
  console.log('I run everytime.');

  // useEffect()를 활용하면 한번만 실행됨
  useEffect(() => console.log('I run only once.'), []);

  return (
    <div>
      <h1>{counter}</h1>
      <button onClick={onClick}>click me</button>
    </div>
  );
};
```

- useEffect()에는 두가지 매개변수가 있는데, 실행할 함수와 dependency가 있음
- dependency는 state를 입력하게 되는데, state의 변화를 감시하는 기능임
- useEffect()에서 지정된 함수는 해당 state가 변화할때만 실행하게 됨
- 최종 코드 (버튼을 클릭해서 카운트를 증가시키거나 검색창에 값을 입력하는 어플리케이션, 각각의 액션에 따라 서로 다른 함수가 실행되도록 설계함)
  ```jsx
  import PropTypes from 'prop-types';
  import styles from './Button.module.css';

  const Button = ({ text, onClick }) => {
    return (
      <button onClick={onClick} className={styles.btn}>
        {text}
      </button>
    );
  };

  Button.propTypes = {
    text: PropTypes.string.isRequired,
  };

  export default Button;
  ```
  ```jsx
  import { useState, useEffect } from 'react';
  import Button from './Button';

  function App() {
    const [counter, setCounter] = useState(0);
    const [keyword, setKeyword] = useState('');

    useEffect(() => console.log('I run only once.'), []);
    useEffect(() => console.log('I run counter changes'), [counter]);
    useEffect(() => console.log('I run keyword changes.'), [keyword]);
    useEffect(() => console.log('I run counter or keyword changes.'), [counter, keyword]);

    const onChange = (event) => setKeyword(event.target.value);
    const onClick = (event) => setCounter((prevCounter) => prevCounter + 1);

    return (
      <div>
        <h1>{counter}</h1>
        <input onChange={onChange} type='text' value={keyword} />
        <Button onClick={onClick} text='Click Me' />
      </div>
    );
  }

  export default App;
  ```

### Cleanup Function

- 컴포넌트가 destroy상태가 될때 실행하는 함수가 있음
- useEffect()의 dependency를 지정하지 않았을때 컴포넌트의 렌더링 실행시 한번만 적용되는 함수를 만들수 있었지만, 이번에는 그 반대로 제거될때 실행되는 함수를 만들수 있다는 이야기
- 샘플 코드
  ```jsx
  // useEffect()가 함수를 리턴할때는 컴포넌트가 제거되었을 때 실행됨
  useEffect(() => {
    console.log('created :)');
    return () => console.log('destroyed :(');
  }, []);
  ```
  ```jsx
  // 이해가 쉽도록 함수를 divide 해보자
  const destroyFn = () => {
    console.log('destroyed!');
  };
  const createFn = () => {
    console.log('created!');
    return destroyFn;
  };
  useEffect(createdFn, []);
  ```
