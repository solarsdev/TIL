## Git Branch

### Branch를 써야 하는 이유가 뭘까?

[Learn Git Branching](https://learngitbranching.js.org/?locale=ko)

- 일반적으로 깃을 초기화 하게 되면 master 브랜치를 기본으로 이용하게 됨
- master 브랜치에는 프로덕트의 검증된 파트(소스코드)만을 저장하는것이 원칙이 됨
- 프로덕트에 새로운 기능을 추가할 때, master 브랜치에서 바로 작업을 하기 보다는 master 브랜치에서 파생된 새로운 브랜치를 이용하는것이 좋음
  - 이러한 방식은 여러명의 개발자가 협동 개발을 해 나갈때 활용되면, 서로 다른 기능은 병렬적으로 업무를 수행할 수 있다는 장점이 있음 (기능단위 브랜치)
- master 브랜치에서 파생된 기능별 브랜치는 테스트가 완료되고 릴리즈 단계에서 master 브랜치에 병합시킴
  - 병합시킬때 다양한 방법을 생각해볼 수 있음
  1. 기능별 파생된 브랜치에서 발생한 커밋을 전부 master 브랜치에 병합할 것인가?
  2. 기능별 파생된 브랜치를 하나의 커밋으로 정리한 다음, 해당 커밋을 master 브랜치에 병합할 것인가?
  - 각각에는 회사별 선호방식이 있기 때문에, 이는 선택사항이 됨
- Git에서 사용하는 Branch의 특장점
  - Git은 브랜치를 새로 만든다고 하더라도, 변경되지 않은 파일이나 원본 스냅샷 (master 브랜치)의 파일로의 스냅샷 포인터만 관리하기 때문에, 브랜치를 작성해도 파일을 일일히 복사해 오는것이 아님
  - 이러한 구조는 브랜치들간의 전환속도가 빠른 핵심이유가 됨

### branch

```bash
git branch [--color[=<when>] | --no-color] [--show-current]
	[-v [--abbrev=<n> | --no-abbrev]]
	[--column[=<options>] | --no-column] [--sort=<key>]
	[--merged [<commit>]] [--no-merged [<commit>]]
	[--contains [<commit>]] [--no-contains [<commit>]]
	[--points-at <object>] [--format=<format>]
	[(-r | --remotes) | (-a | --all)]
	[--list] [<pattern>…]
git branch [--track[=(direct|inherit)] | --no-track] [-f] <branchname> [<start-point>]
git branch (--set-upstream-to=<upstream> | -u <upstream>) [<branchname>]
git branch --unset-upstream [<branchname>]
git branch (-m | -M) [<oldbranch>] <newbranch>
git branch (-c | -C) [<oldbranch>] <newbranch>
git branch (-d | -D) [-r] <branchname>…
git branch --edit-description [<branchname>]
```

- 브랜치 목록을 확인, 생성, 삭제함
- `—list` 혹은 아무런 옵션을 입력하지 않으면 현재 브랜치들의 목록이 표시됨
- 현재 브랜치는 \* 마크가 표시된 상태로 녹색으로 하이라이트됨
- 링크된 작업트리에서 체크아웃된 모든 브랜치는 + 마크가 표시된 상태로 cyan으로 하이라이트됨
- 옵션으로 `-r` 을 지정하면 리모트 브랜치들을 표시하며, `-a` 옵션은 로컬과 리모트에서 모든 브랜치를 표시함
- 패턴(\*기호를 통해서 와일드카드 문자열)이 주어지면 매치되는 브랜치들을 표시함
  - 패턴은 여러개 입력가능하며, 패턴검색으로 찾아낸 브랜치들의 합집합이 표시됨
- 브랜치를 생성하려면 `-c / -C / -m / -M` 옵션을 이용
- 브랜치를 삭제하려면 `-d / -D` 옵션을 이용
- `—merged` 옵션은 HEAD가 가리키는 커밋에 병합된 모든 브랜치를 표시
- `—no-merged` 옵션은 HEAD가 가리키는 커밋에 병합되지 않은 모든 브랜치를 표시
