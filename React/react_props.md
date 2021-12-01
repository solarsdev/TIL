## React Props

- React에서 각각의 기능요소(간단헌 버튼부터 다기능을 포함한 컨버터 등)를 함수형 컴포넌트로 만들어서 호출하는 기능을 구현하고, 구현된 컴포넌트를 호출할때 데이터를 전달할 수 있음
- 이 데이터를 React Properties, 줄여서 Props라고 부름
- Props의 활용방법은 여러가지 패턴이 있는데 특정 컴포넌트의 구성요소가 대부분 동일한 상황에서 조건에 따라 스타일을 변경한다거나, 문자열 일부를 입력받아 출력하는 경우 등이 있음
    - 샘플 코드 (버튼 컴포넌트를 정의하고, 버튼에 표시할 텍스트와 크기 스타일링을 Props로 처리)
        
        ```html
        <body>
          <div id="root"></div>
        </body>
        ```
        
        ```jsx
        const Btn = ({ text }) => {
          return (
            <button
              style={{
                backgroundColor: 'tomato',
                color: 'white',
                padding: '10px 20px',
                borderRadius: '4px',
                border: 0,
                margin: '10px',
              }}>
              {text}
            </button>
          );
        };
        const App = () => {
          return (
            <div>
              <Btn text='Save Changes' />
              <Btn text='Confirm' />
            </div>
          );
        };
        const Root = document.getElementById('root');
        ReactDOM.render(<App />, Root);
        ```
        

### React Memo

- React에서 컴포넌트의 State가 변경되면, 해당 컴포넌트가 그리고 있는 모든 렌더링 항목들이 새로고침 되는데, 이러한 특성으로 인해 부모 컴포넌트가 많은 자식 컴포넌트들을 소유하고 있다면 퍼포먼스 문제가 될 수 있음
- 이를 해결하기 위한 방법으로는, 컴포넌트의 props 상태를 저장하는 Memo기능을 이용하여 해당 시점을 저장해두고, 그 이후로 props가 변경되는 컴포넌트만 새로고침 하는 방식을 이용할 수 있음
- 메모이징에 대한 자세한 설명은 다음 블로그에서 참조
    
    [[React] React.memo() 언제 사용하지?](https://velog.io/@qwe6293/React-React.memo-%EC%96%B8%EC%A0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80)
    
- 메모이징을 활용한 샘플코드
    
    ```html
    <body>
      <div id="root"></div>
    </body>
    ```
    
    ```jsx
    const Btn = ({ text, onClickSaveChanges }) => {
      console.log(`${text} rendered!`);
      return (
        <button
          onClick={onClickSaveChanges}
          style={{
            backgroundColor: 'tomato',
            color: 'white',
            padding: '10px 20px',
            borderRadius: '4px',
            border: 0,
            margin: '10px',
          }}>
          {text}
        </button>
      );
    };
    const MemoizedBtn = React.memo(Btn);
    const App = () => {
      const [value, setValue] = React.useState('Save Changes');
      const onClickSaveChanges = () => setValue('Revert Changes');
      return (
        <div>
          <MemoizedBtn text={value} onClickSaveChanges={onClickSaveChanges} />
          <MemoizedBtn text='Confirm' />
        </div>
      );
    };
    const Root = document.getElementById('root');
    ReactDOM.render(<App />, Root);
    ```
    
    <aside>
    💡 Memo기능을 이용하면 컴포넌트의 Props 변경을 감지하여, 변경이 있을 경우에만 새로고침을 하게 됨
    
    </aside>
    

### React PropTypes

- React에서는 컴포넌트의 Prop을 입력받고, 해당 데이터를 컴포넌트 내에서 활용하게 되는데 자바스크립트의 특성상 입력값이 항상 원하는 타입으로 들어온다는 보장이 없고, 값이 없을수도 있음
- 이러한 경우에는 타입스크립트처럼 컴파일 단계에서 에러처리를 해주지는 못하지만, React의 PropTypes를 이용하면 실행단계에서 콘솔 에러 메시지를 확인할수 있도록 편의를 제공함
- PropTypes Document
    
    [PropTypes와 함께 하는 타입 검사 - React](https://ko.reactjs.org/docs/typechecking-with-proptypes.html)
    
- PropTypes 기능을 이용해서 버튼 컴포넌트에 입력되는 Prop의 타입 및 필수여부를 정의함
    
    ```html
    <body>
      <div id="root"></div>
    </body>
    ```
    
    ```jsx
    const Btn = ({ text, fontSize = 16 }) => {
      console.log(`${text} rendered!`);
      return (
        <button
          style={{
            backgroundColor: 'tomato',
            color: 'white',
            padding: '10px 20px',
            borderRadius: '4px',
            border: 0,
            margin: '10px',
            fontSize,
          }}>
          {text}
        </button>
      );
    };
    Btn.propTypes = {
      text: PropTypes.string.isRequired,
      fontSize: PropTypes.number,
    };
    const App = () => {
      return (
        <div>
          <Btn text='Save Changes' fontSize='18' />
          <Btn text='Confirm' />
        </div>
      );
    };
    const Root = document.getElementById('root');
    ReactDOM.render(<App />, Root);
    ```