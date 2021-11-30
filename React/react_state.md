## React State

- React에서 State란 컴포넌트가 가지고 있는 상태를 의미하며, 객체로 관리되고 컴포넌트 내에서만 정의되고 사용 가능함
- Basics에서 구성해보았던 [counter]와 같은 변수들이 바로 State로 정의할수 있는 부분

### State의 구현

- State를 구현하기에 앞서, 먼저 React에서 어떤 방식으로 컴포넌트가 리프레쉬 되는지 파악해야함
  - 기본 코드 (버튼을 눌러서 클릭수를 증가시키는 어플리케이션)
    ```html
    <body>
      <div id="root"></div>
    </body>
    ```
    ```jsx
    const Root = document.getElementById('root');
    let counter = 0;
    const handleClick = () => {
      counter++;
      render();
    };
    const render = () => {
      ReactDOM.render(<App />, Root);
    };
    const App = () => (
      <div>
        <h3>Total Clicks: {counter}</h3>
        <button onClick={handleClick}>Click Me</button>
      </div>
    );
    render();
    ```
- React에서 웹페이지 요소를 업데이트 할때 사용하는 특별한 함수가 있음
  ```jsx
  RenderDOM.render(<Container />, Root);
  ```
    <aside>
    💡 render함수는 특정 컴포넌트를 화면에 다시 그리는 역할을 하는데, 상기 코드에서는 카운터값을 증가시킬때마다 렌더링을 새로 해주고 있음. 렌더링을 할때는 모든 페이지가 아니라 바뀐 부분만 업데이트가 되므로 이 부분은 매우 효율적이라고 판단됨. 다만 특정 값을 업데이트 할때마다 render를 호출하는게 과연 어떨지? 이 부분에 대한 고민이 필요
    
    </aside>

- 특정값을 업데이트 할때마다 자동으로 리렌더링을 하는 방법 (State를 이용)
  ```jsx
  const data = React.useState();
  // => [undefined, function]
  // useState의 초기값과 더불어 초기값에 접근할수 있는 함수를 가지게 됨

  // array destructuring을 이용해서 초기값, 함수에 변수명명이 가능
  const [counter, modifier] = React.useState(0);

  // modifier에 값을 주면 초기값을 갱신한다
  modifier(10); // counter = 10
  // +게다가 관련 컴포넌트의 리렌더링도 자동으로 진행됨 👍
  ```
- 리렌더링을 한다고 하더라도 최종적으로는 변경될 부분만 렌더링을 새로 고치기 때문에 효율성은 유지됨
  - 최종 코드
    ```html
    <body>
      <div id="root"></div>
    </body>
    ```
    ```jsx
    const App = () => {
      const [counter, setCounter] = React.useState(0);
      const handleClick = () => {
        setCounter(counter + 1);
      };

      return (
        <div>
          <h3>Total Clicks: {counter}</h3>
          <button onClick={handleClick}>Click Me</button>
        </div>
      );
    };
    const Root = document.getElementById('root');
    ReactDOM.render(<App />, Root);
    ```
      <aside>
      💡 React 본연의 기능을 활용하면서 이제 좀 코드가 가벼워진 느낌
      
      </aside>


### React에서 State를 설정하는 2가지 방법

```jsx
const [counter, setCounter] = React.useState(0);
// counter: value
// setCounter: modifier
setCounter(counter + 1);
```

- counter라는 변수를 state로 지정하여 관리하는 방법으로는 상기 코드가 있을수 있는데, 이 방식을 사용하게 되면 **counter의 값이 원하는대로 나오지 않을 가능성**이 있음
  - 어떤 경우에 원하는대로 값이 나오지 않을까?
    [【React】そろそろ技術ブログで setCount(count + 1) と書くのはやめませんか](https://zenn.dev/stin/articles/use-appropriate-api)
  - 결론부터 이야기하면 같은 컴포넌트 내에서 setCounter에서 변수를 직접 호출하면 동일한 counter값을 가져온다는 것임
    ```jsx
    const [counter, setCounter] = React.useState(0);
    // counter: value
    // setCounter: modifier
    setCounter(counter + 1);
    setCounter(counter + 1);
    ```
  - 상기 코드에서는 setCounter를 2번 사용한다고 해서 값이 2씩 증가하는것이 아니라 실제로는 1씩 증가하게 된다. 즉, counter는 read-only형태로서 실질적으로 counter값을 이용하기 위해서는 다른 방식을 이용한다.
    ```jsx
    // new way to modify counter
    setCounter((prevValue) => prevValue + 1);
    // 결론은 counter와 같은 state값이 접근할때는,
    // modifier의 parameter를 이용해야 한다는 것을 명심하자
    ```
- 결론, state를 설정하는 2가지 방법이 있음
  1. modifier를 통해 상수값을 직접 설정하는 방법
  2. 직전 state값을 이용해서 증감을 하는 방법

### React에서 Input 관리 (State를 이용)

- React에서 State를 이용하면 Input에서 입력받은 데이터를 컴포넌트 내부의 값으로 가지고 갈 수 있음
- 컴포넌트에서 State를 선언한 뒤, Input의 onChange와 같은 이벤트를 이용해서 value를 가지고 오자
- 샘플 어플리케이션을 작성
  - 시간, 분을 입력받고 시간이 입력됐다면 분을, 분이 입력됐다면 시간을 자동으로 변환하여 출력해주는 어플리케이션 (Super Converter)
    ```html
    <body>
      <div id="root"></div>
    </body>
    ```
    ```jsx
    const MinutesToHours = () => {
      const [minutes, setMinutes] = React.useState(0);
      const [hours, setHours] = React.useState(0);
      const [flipped, setFlipped] = React.useState(false);
      const onMinutesChange = (event) => {
        const numMinutes = event.target.value;

        setMinutes(numMinutes);
        setHours(Math.round(numMinutes / 60));
      };
      const onHoursChange = (event) => {
        const numHours = event.target.value;

        setMinutes(numHours * 60);
        setHours(numHours);
      };
      const onFlip = () => {
        setFlipped((prevFlipped) => !prevFlipped);
      };
      const reset = () => {
        setMinutes(0);
        setHours(0);
      };
      return (
        <div>
          <h2>Minutes to Hours</h2>
          <label htmlFor='minutes'>Minutes</label>
          <input
            id='minutes'
            type='number'
            placeholder='Minutes'
            onChange={onMinutesChange}
            value={minutes}
            disabled={!flipped}
          />
          <label for='hours'>Hours</label>
          <input
            id='hours'
            type='number'
            placeholder='Hours'
            onChange={onHoursChange}
            value={hours}
            disabled={flipped}
          />
          <button onClick={reset}>Reset</button>
          <button onClick={onFlip}>Flip</button>
        </div>
      );
    };
    const KilometersToMiles = () => {
      const [kilometer, setKilometer] = React.useState(0);
      const [mile, setMile] = React.useState(0);
      const [flipped, setFlipped] = React.useState(false);
      const onKilometerChange = (event) => {
        const numKilometer = event.target.value;

        setKilometer(numKilometer);
        setMile(numKilometer * 0.6214);
      };
      const onMileChange = (event) => {
        const numMile = event.target.value;

        setKilometer(numMile / 0.6214);
        setMile(numMile);
      };
      const onFlip = () => {
        setFlipped((prevFlipped) => !prevFlipped);
      };
      const reset = () => {
        setKilometer(0);
        setMile(0);
      };
      return (
        <div>
          <h2>Kilometers To Miles</h2>
          <label htmlFor='kilometer'>Kilometer</label>
          <input
            id='kilometer'
            type='number'
            placeholder='Kilometers'
            onChange={onKilometerChange}
            value={kilometer}
            disabled={!flipped}
          />
          <label for='mile'>Miles</label>
          <input
            id='mile'
            type='number'
            placeholder='Miles'
            onChange={onMileChange}
            value={mile}
            disabled={flipped}
          />
          <button onClick={reset}>Reset</button>
          <button onClick={onFlip}>Flip</button>
        </div>
      );
    };
    const App = () => {
      const [menuIndex, setMenuIndex] = React.useState('0');
      const onSelectChange = (event) => {
        setMenuIndex(event.target.value);
      };
      return (
        <div>
          <h1>Super Converter</h1>
          <select onChange={onSelectChange}>
            <option value='0'>Select converter...</option>
            <option value='1'>Minutes to Hours</option>
            <option value='2'>Kilometers to Miles</option>
          </select>
          {menuIndex === '0' ? <h2>Please select converter you want</h2> : null}
          {menuIndex === '1' ? <MinutesToHours /> : null}
          {menuIndex === '2' ? <KilometersToMiles /> : null}
        </div>
      );
    };
    const Root = document.getElementById('root');
    ReactDOM.render(<App />, Root);
    ```
      <aside>
      💡 React에서의 State관리와 더불어 사용자의 입력을 State로 관리하는 방법을 익히고, JSX를 이용해서 컴포넌트를 조건에 따라서 출력해보는 예제를 만들어봄
      
      </aside>
