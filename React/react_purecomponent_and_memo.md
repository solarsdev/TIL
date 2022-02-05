# ⚡️PureComponent와 Memo

## ❗️React 성능 이야기

- React 어플리케이션은 컴포넌트들로 구성되어 있고, **React 내부 데이터인 state에 변경이 생길 경우에 영향을 받는 모든 컴포넌트들과 요소들을 자동으로 업데이트** 해주기 때문에, 개발자가 직접 데이터 갱신을 해 줄 필요가 없다.
- 분명 이렇게 자동으로 업데이트를 해주는 편리함으로 인해 개발이 쉽고 재밌다는것은 장점이지만, **잘못된 코딩으로 인해서 개발자가 예상하는 범위를 벗어난 컴포넌트까지 업데이트 되고 있다면?** 수동으로 업데이트 해주는것만 못한 성능이 나올수도 있다는 이야기가 된다.
- 다만, React는 그렇게 바보같지는 않아서 `Virtual DOM Tree`로 엘리먼트들을 트리 형태로 메모리에 저장하고 있으며, **데이터가 변경되어서 컴포넌트내 어떤 엘리먼트들이 업데이트 되는지를 계산한 뒤에 정말 변경이 필요한 경우에만 엘리먼트를 업데이트** 하기 때문에, 렌더링 측면에서는 그렇게까지는 영향을 받는 편이 아니다.
- 하지만, 계산을 위해서는 실제로 컴포넌트를 업데이트 해본 다음 결과값을 비교해야 하기 때문에, `render()` 함수는 실제로 호출이 되며, 이 경우에 컴포넌트 내에서 다양한 라이프 사이클 메소드를 이용해서 처리 (예를 들어, 백엔드 API의 호출을 통해 데이터를 받아온다거나) 등이 의도치 않게 발생할 여지가 있다.

## ❗️컴포넌트 로직의 중복 호출을 방지하기 위해서는 어떻게 해야 할까?

### 👉React.PureComponent를 사용

리액트 공식문서에서는 `React.PureComponent`에 대해서 다음과 같이 설명하고 있다.

> `React.PureComponent`는 `React.Component`와 비슷합니다. `React.Component`는 `shouldComponentUpdate()`를 구현하지 않지만, `React.PureComponent`는 props와 state를 이용한 얕은 비교를 구현한다는 차이점만이 존재합니다.

> React 컴포넌트의 `render()` 함수가 동일한 props와 state에 대하여 동일한 결과를 렌더링한다면, `React.PureComponent`를 사용하여 경우에 따라 성능 향상을 누릴 수 있습니다.

- 공식문서에서 설명하는 말을 풀어보면 `Component`와 `PureComponent`의 차이점이 무엇인지 알 수 있다.
  - `shouldComponentUpdate()`의 구현 유무
  - 구현된 `shouldComponentUpdate()`의 비교 로직이 **shallow comparison**이라는 것
- 결론은 shallow한 비교루틴을 가지고 있는 PureComponent의 경우에는 경우에 따라서는 `render()` 함수의 호출 빈도가 줄어들 수 있기 때문에, 성능 향상을 누릴 수 있다는 뜻이다.

## ❗️shouldComponentUpdate()의 구현 유무란?

- 특정 조건하에서 컴포넌트를 다시 렌더링 할것인지 아닌지의 유무를 결정하는 것이다. (다시 말하면 `render()` 함수를 호출할지 말지를 결정한다는 뜻)

## ❗️그럼 그 조건인 shallow comparison이 대체 뭐야?

- 오브젝트의 값이 동일한지 아닌지 확인할때 레퍼런스 값을 가지고 있는지 비교하는것
- 즉, 오브젝트를 참조하는지 아닌지만 확인하고, 오브젝트 내부의 키까지 전부 참조하여 값을 비교하지는 않는다는 의미
- 오브젝트 내부의 키까지 전부 참조하여 값을 가져와서 비교하는것은 `deep comparison`이라고 불린다.

## Reference

[React 최상위 API - React](https://ko.reactjs.org/docs/react-api.html#reactpurecomponent)

[리액트 강의 (유튜브 클론 코딩 + 실시간 전송 명함 카드 만들기 웹앱 만들기)](https://academy.dream-coding.com/courses/react-basic)
