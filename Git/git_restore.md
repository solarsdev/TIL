## restore

```jsx
git restore [<options>] [--source=<tree>] [--staged] [--worktree] [--] <pathspec>…
git restore [<options>] [--source=<tree>] [--staged] [--worktree] --pathspec-from-file=<file> [--pathspec-file-nul]
git restore (-p|--patch) [<options>] [--source=<tree>] [--staged] [--worktree] [--] [<pathspec>…]
```

- `working directory`의 파일을 기존 소스로부터 복원, 만일 지정된 소스의 원본에 존재하지 않는 기존 파일은 삭제됨
- —staged 옵션을 통해 인덱스된 공간의 파일도 복원 가능하고, —staged, —worktree 옵션을 둘 다 사용하는 것으로 양쪽 공간의 파일을 동시에 복원하는 것도 가능
- —staged 옵션이 붙으면 HEAD가 가리키고 있는 소스로부터 복원되며, 워킹 디렉토리의 복원은 스테이징된 파일로부터 복원됨, —source 옵션을 이용해서 특정 커밋으로 부터의 복원도 가능

## commit —amend

- 현재 브랜치의 새로운 커밋을 작성해서 기존 커밋을 덮어씌움
- 기존 커밋에서 새로운 내용을 추가하거나 제거한 뒤에 메시지도 새롭게 고칠 수 있음
- 어디까지나 새로운 커밋을 작성하는 것이기 때문에, 이미 리모트 저장소에 푸시가 되어 있을 경우에는 강제 업데이트가 필요함
