# ⚡️Ref와 DOM

리액트 공식 문서에서는 Ref에 대해서 다음과 같이 설명하고 있다.

> 일반적인 데이터 플로우에서 벗어나 직접적으로 자식을 수정해야 하는 경우도 가끔씩 있습니다. 수정할 자식은 React 컴포넌트의 인스턴스일 수도 있고, DOM 엘리먼트일 수도 있습니다. React는 두 경우 모두를 위한 해결책을 제공합니다.

## ❗️공식문서에 말하는 일반적인 데이터 플로우란 무엇일까?

- 리액트에서는 부모 컴포넌트와 자식과의 상호작용은 `props`를 이용한다.
- 자식을 수정하려면 부모는 새로운 `props`를 전달하여 자식을 다시 렌더링 해야 한다.

## ❗️DOM 엘리먼트를 참조하려면?

- 개발을 진행하다가 DOM 엘리먼트를 직접 제어해야 하는 경우가 발생할 수 있다.

### 👉포커스, 텍스트 선택영역, 혹은 미디어의 재생을 관리할 때

- form 에서 submit 버튼을 눌렀을때, 특정 input 엘리먼트에 있는 value 값을 가져오는 경우
- 음악 파일의 재생 또는 중지를 위해 audio 엘리먼트를 제어해야 하는 경우

### 👉애니메이션을 직접적으로 실행시킬 때

### 👉서드 파티 DOM 라이브러리를 React와 같이 사용할 때

## ❗️Ref 생성하기

- Ref는 React.createRef()를 통해 생성되고 ref 속성을 통해 React 엘리먼트에 부착된다.
- 일반적으로 컴포넌트의 멤버변수에 Ref를 할당하여 컴포넌트의 어떤 곳에서도 Ref에 접근할 수 있도록 한다.

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

## Reference

[Ref와 DOM - React](https://ko.reactjs.org/docs/refs-and-the-dom.html)

[[React] ref로 HTML 엘리먼트에 접근/제어하기](https://www.daleseo.com/react-refs/)
