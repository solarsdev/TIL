## Stash에 대해서

### stash를 사용해야 하는 경우

- Git에는 working tree, staging area, .git directory의 개념으로 버전관리를 하게 되는데, 이때 working tree에서 편집을 진행하던 중 다른 브랜치로 checkout하여 작업을 진행해야 할 경우가 있음 (테스팅, 내용 수정 등등)
- 기존의 working tree의 내용을 커밋한 뒤에 checkout이 가능하다면 좋겠지만, 작업 내용이 어중간할 경우 곤란한 상황에 처할수 있음
- 이런 경우에는 Git에서 제공하는 stash기능을 이용하면 되는데, stash는 modified이면서 tracked 상태인 파일과 staging area에 있는 파일들을 보관해두는 장소로서 아직 끝내지 않은 수정사항을 스택에 잠시 저장했다가 나중에 다시 적용할 수 있음 (적용할때 꼭 원래 브랜치에서 적용해야 하는것은 아님)
- stash를 다른 브랜치나 깨끗하지 않은 working tree에 적용할수 있다는것은 내용에 충돌이 발생할수 있다는것을 의미함

### stash

```bash
git stash list [<log-options>]
git stash show [-u|--include-untracked|--only-untracked] [<diff-options>] [<stash>]
git stash drop [-q|--quiet] [<stash>]
git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
git stash branch <branchname> [<stash>]
git stash [push [-p|--patch] [-S|--staged] [-k|--[no-]keep-index] [-q|--quiet]
	     [-u|--include-untracked] [-a|--all] [-m|--message <message>]
	     [--pathspec-from-file=<file> [--pathspec-file-nul]]
	     [--] [<pathspec>…]]
git stash clear
git stash create [<message>]
git stash store [-m|--message <message>] [-q|--quiet] <commit>
```

- `working directory`의 변경사항을 저장하고 전부 삭제한다.
- 이 명령은 정확하게는 변경사항을 전부 스택형 저장공간에 보존해둔 뒤, HEAD와 일치하도록 변경사항을 돌리는것을 이야기 한다.
- 이 명령으로 숨긴 수정 사항은 `git stash list`로 확인하고, `git stash show`로 열어보고, `git stash apply`로 복원(다른 커밋상에서도 수행할 수 있음)할 수 있다.
- 인수 없이 `git stash`를 호출하는 것은 `git stash push`와 동일하다.
- 가장 최근에 생성한 `stash`는 `refs/stash`에 저장되며, `reflogs`의 명명규칙을 이용하면 `stash`에 직접 이름을 지어줄 수도 있다. (예를 들면, `stash@{0}`는 가장 최근에 작성된 stash이고, `stash@{1}`는 바로 직전에 만들어진 stash인데, `stash@{2.hours.ago}`또한 명명규칙에 따라 가능하다.)
