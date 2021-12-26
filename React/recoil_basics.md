## Recoil

- 리코일은 React에서 발생하는 이러한 상태전달 문제를 해결하기 위해 등장한 라이브러리이며, 리코일에서 사용하는 상태저장소를 Atom이라고 부름
- 각각의 atom에는 각기 다른 값을 저장할 수 있음
- 만들어진 atom은 각각의 컴포넌트와 연결하고 바로 사용 가능함
- 이러한 개념을 통해, 글로벌 State의 관리가 가능해짐
- Recoil의 설치 및 이용 방법

  1. npm으로 recoil을 설치

     ```bash
     npm install recoil
     ```

  2. App을 RecoilRoot로 감싸줌

     ```tsx
     <RecoilRoot>
       <App />
     </RecoilRoot>
     ```

  3. atoms.ts 작성

     ```tsx
     import { atom } from 'recoil';

     export const themeNameAtom = atom({
       key: 'themeName',
       default: 'light',
     });
     ```

  4. useRecoilValue()를 통해서 값을 받아서 사용

     ```tsx
     const themeName = useRecoilValue(themeNameAtom);
     ```

  5. useSetRecoilState()를 통해서 값을 설정

     - useStateRecoilState()는 해당 atom의 setter function을 리턴

     ```tsx
     const setThemeName = useSetRecoilState(themeNameAtom);
     const toggleTheme = () =>
       setThemeName((prevTheme) => (prevTheme === 'light' ? 'dark' : 'light'));

     <PaletteBtn onClick={toggleTheme}>
       <FontAwesomeIcon icon={faPalette} />
     </PaletteBtn>;
     ```

- Atom 또한 State의 일부이며, Atom을 사용한다는 것은 해당 State를 구독한다는 뜻이기 때문에, Atom에 어떤 값의 변화가 있을 경우에는 컴포넌트가 다시 렌더링 된다는 것을 의미함
  - 좋은점은 자동으로 변경된 값을 반영할수 있음—
- redux, recoil 내용 정리
  [redux, recoil 내용 정리](https://velog.io/@katanazero86/redux-recoil-%EB%82%B4%EC%9A%A9-%EC%A0%95%EB%A6%AC)
- 정리
  - 상태관리 라이브러리는 Traveling Props 문제를 해결함
  - 상태관리 라이브러리 중 Recoil을 사용, Recoil은 Atom이라는 글로벌 State를 이용해서 상태를 관리함
  - Recoil의 사용법
    - useRecoilValue()
    - useSetRecoilState()
