### Typing the Props

- 컴포넌트에 타입을 지정하는 방법
- JavaScript에서 PropTypes를 통해 React의 Props들의 타입을 확인하는 기능을 추가할 수 있지만, PropTypes는 JavaScript 라이브러리이므로, 런타임에서 확인후 브라우저 콘솔에 출력하는 정도에 그쳤음
- TypeScript를 사용하면, 코드를 작성하는 시점에서 확인후 에러를 표시하므로, 오류가 있는 코드를 릴리즈 하지 않도록 도와줌
- Interface
  - 인터페이스란 객체의 생김새를 TypeScript에서 설명해주는 개념
  - 컴포넌트의 Props를 사용할때 Props Object를 이용하게 되는데, 그때 Prop들마다 타입을 지정해줄수도 있지만, interface를 이용해서 객체 형태를 먼저 정의해주고 해당 interface를 사용할 수 있음
    ```jsx
    interface ContainerProps {
      bgColor: string;
    }

    const Container =
      styled.div <
      ContainerProps >
      `
      width: 200px;
      height: 200px;
      background-color: ${(props) => props.bgColor};
      border-radius: 100px;
    `;

    interface CircleProps {
      bgColor: string;
    }

    const Circle = ({ bgColor }: CircleProps) => {
      return <Container bgColor={bgColor} />;
    };
    ```
      <aside>
      💡 스타일 컴포넌트에서 props를 정의할때 <Props>형태로 사용하는데, 이때 Interface를 이용해서 깔끔하게 코딩 가능
      
      </aside>


### Optional Props

- Typing the Props 섹션에서 작성한 샘플코드의 경우에 Interface를 이용하여 Props를 명시했는데, 이렇게 되면 기본적으로 required 상태가 됨
- Props를 선택적으로 사용하고 싶을 경우에는 어떻게 하면 될까?
- TypeScript에서는 선언형 매개변수, 변수의 선택자에 ?를 붙여서 정의하면 됨

```jsx
interface ContainerProps {
  bgColor: string;
  borderColor: string;
}

const Container =
  styled.div <
  ContainerProps >
  `
  width: 200px;
  height: 200px;
  background-color: ${(props) => props.bgColor};
  border-radius: 100px;
  border: 1px solid ${(props) => props.borderColor};
`;

interface CircleProps {
  bgColor: string;
  borderColor?: string;
}

const Circle = ({ bgColor, borderColor }: CircleProps) => {
  return <Container bgColor={bgColor} borderColor={borderColor ?? bgColor} />;
};
```

- borderColor를 선택적으로 사용하고 싶을 경우 borderColor?: string으로 정의
- JavaScript에서 A ?? B 연산자는 A가 null 또는 undefined일 경우에 B가 실행되는 연산자
  [](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
- 상기 코드의 해석으로는 Container의 Props중 bgColor는 필수이며, borderColor는 선택적이며, 값이 Props Data로 입력될 경우에는 해당 값을 취하지만, 그렇지 않을 경우에는 bgColor와 동일한 색상으로 border를 작성하게 됨
