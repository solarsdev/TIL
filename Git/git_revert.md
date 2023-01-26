## revert

- 존재하는 커밋을 취소하는 작업을 만들기

```bash
git revert [--[no-]edit] [-n] [-m <parent-number>] [-s] [-S[<keyid>]] <commit>…
git revert (--continue | --skip | --abort | --quit)
```

- 현재 존재하는 커밋들을 전달하여 해당 커밋에서 변경된 사항을 되돌리는 작업을 생성
- 현재 작업중인 워킹 트리를 클린하게 해야 할 필요가 있음 (HEAD 커밋과의 변경점이 없어야 함)

<aside>
💡 git revert는 기존의 변경점을 취소하는 새로운 커밋을 만드는 것, 커밋되지 않은 변경점들을 날리고 싶으면 git reset을, 특정 파일을 지정하여 복원하고 싶을 경우에는 git restore를 사용함

</aside>
