## Recoil Selector

- Recoil에는 Selector라는 개념이 있음
- 간단하게 이야기하면, Recoil은 전역 상태를 atom이라는 개념으로 관리하는데, selector는 atom에 함수적 처리를 통해 state를 쪼개는것을 말함
- state를 쪼개지 않고 특정 컴포넌트에서 atom 전체를 받아서 사용해버리면, atom에 상태변화가 생겼을때 컴포넌트 내에서 atom의 일부분만 이용하고 있다고 하더라도, 전부 영향을 받아버리게 됨
- 따라서, atom을 selector를 이용해서 쪼개주는것은 중요하며, to do 어플리케이션으로 예를 들면 to do안의 카테고리 (todo, doing, done)등을 selector로 분리시켜주는 것임
- selector를 사용한 또다른 이점으로는 selector는 기본적으로 캐싱을 지원한다는 것인데, 이에 대한 자세한 설명을 해놓은 블로그를 발견했으니 참고하자
  [주요 개념 | Recoil](https://recoiljs.org/ko/docs/introduction/core-concepts#selectors)
  [[Recoil] Selector를 이용하여 API 값 캐싱하기 with React & TypeScript 📮](https://velog.io/@yiyb0603/Selector%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%98%EC%97%AC-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EA%B0%92-%EC%BA%90%EC%8B%B1%ED%95%98%EA%B8%B0)
- toDo 어플리케이션에서 selector를 이용해서 atom을 분리한 샘플코드

  ```tsx
  // atom.ts

  import { atom, selector } from 'recoil';
  import ToDo from './interfaces/ToDo';

  export const toDoState = atom<ToDo[]>({
    key: 'toDo',
    default: [],
  });

  export const toDoSelector = selector({
    key: 'toDoSelector',
    get: ({ get }) => {
      const toDos = get(toDoState);
      return [
        toDos.filter((toDo) => toDo.category === 'TO_DO'),
        toDos.filter((toDo) => toDo.category === 'DOING'),
        toDos.filter((toDo) => toDo.category === 'DONE'),
      ];
    },
  });
  ```

  ```tsx
  // ToDoList.tsx

  import { useRecoilValue } from 'recoil';
  import { toDoSelector } from '../atoms';
  import BaseToDo from './BaseToDo';
  import CreateToDo from './CreateToDo';

  const ToDoList = () => {
    const [toDos, doings, dones] = useRecoilValue(toDoSelector);
    return (
      <div>
        <CreateToDo />
        <hr />
        <h1>To Do</h1>
        <ul>
          {toDos.map((toDo) => (
            <BaseToDo key={toDo.id} {...toDo} />
          ))}
        </ul>
        <hr />
        <h1>Doing</h1>
        <ul>
          {doings.map((doing) => (
            <BaseToDo key={doing.id} {...doing} />
          ))}
        </ul>
        <hr />
        <h1>Done</h1>
        <ul>
          {dones.map((done) => (
            <BaseToDo key={done.id} {...done} />
          ))}
        </ul>
      </div>
    );
  };

  export default ToDoList;
  ```
