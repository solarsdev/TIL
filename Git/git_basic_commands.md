## Git의 기본 명령어

### add (git에 파일 추가하기)

```bash
git add [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
	  [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]] [--sparse]
	  [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing] [--renormalize]
	  [--chmod=(+|-)x] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	  [--] [<pathspec>…]
```

- add 명령어는 기본적으로는 working tree에서 대상의 인덱스를 업데이트 하는데 사용함
- 인덱스라는 것은 commit에서 사용되는 스냅샷(변경사항의 일부분)의 해시키를 의미하며, git을 통한 파일관리의 핵심 컨셉임
- add 명령어는 commit이 수행되기 전에 여러번 수행할수 있으며, 일반적으로는 개별 파일의 전체 경로를 지정하여 하나의 파일을 대상으로 활용할수도 있고, 경로 자체를 제공하여 모든 파일을 지정하는것도 가능
- commit을 수행할때 대상이 되는 파일은 add로 인덱스가 지정된 파일뿐이기 때문에 commit 전에는 반드시 add로 대상을 지정해야 함
- ignore에 지정된 파일을 대상으로 add를 수행해도 일반적으로는 무시되며, `-f` (force) 옵션을 이용하면 ignore에 등록된 파일이라고 할지라도 강제로 등록하는것이 가능함

### `.gitignore` (git에 추가되지 않는 파일 정의하기)

```bash
$XDG_CONFIG_HOME/git/ignore, $GIT_DIR/info/exclude, .gitignore
```

- 깃에서 추적 대상에서 의도적으로 제외시킬 파일의 경로나 패턴을 정의
- 이미 add를 통해서 추적대상에 지정된 파일은 추후에 ignore에 지정되더라도 제외되지 않음

### status (현재 working tree, staging area의 상태 확인)

```bash
git status [<options>…] [--] [<pathspec>…]
```

- 스냅샷 인덱스를 현재 commit이 가리키고 있는 HEAD(스냅샷 위치, 즉 현재 원본)와 비교하여 다른 점을 명세함
- 첫번째 나오는 정보는 `git commit` 명령어를 이용하면 커밋 가능한 파일들이며, 두번째 나오는 정보는 `git add` 를 이용해서 staging area에 등록하면 `git commit` 명령어를 통해 커밋 가능한 파일임
- 기본적으로는 `git status —long` 옵션이 실행되며, `-s` 옵션을 붙이면 간단한 명세가 표시됨

### diff

```bash
[diff]
	tool = vscode
[difftool "vscode"]
	cmd = code --wait --diff $LOCAL $REMOTE
```

- commit과 working tree와의 차이점을 명세

```bash
git diff [<options>] [<commit>] [--] [<path>…]
git diff [<options>] --cached [--merge-base] [<commit>] [--] [<path>…]
git diff [<options>] [--merge-base] <commit> [<commit>…] <commit> [--] [<path>…]
git diff [<options>] <commit>…<commit> [--] [<path>…]
git diff [<options>] <blob> <blob>
git diff [<options>] --no-index [--] <path> <path>
```

- commit과 `working tree`와의 차이점 혹은 `working tree`와 다른 `working tree`와의 차이점 등을 비교할 수 있는 명령어
- 파일 내 어떤 부분에 변경이 있었는데 상세한 내용을 표시해줌
- 대상이 되는 파일 or 커밋을 지정해서 비교할 수 있음

### commit

- 레포지토리에 변경 사항을 기록함

```bash
git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>] [--amend]
	   [--dry-run] [(-c | -C | --squash) <commit> | --fixup [(amend|reword):]<commit>)]
	   [-F <file> | -m <msg>] [--reset-author] [--allow-empty]
	   [--allow-empty-message] [--no-verify] [-e] [--author=<author>]
	   [--date=<date>] [--cleanup=<mode>] [--[no-]status]
	   [-i | -o] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	   [(--trailer <token>[(=|:)<value>])…] [-S[<keyid>]]
	   [--] [<pathspec>…]
```

- 새로운 커밋에는 현재 인덱스의 변경 사항과 변경 사항에 대한 로그 메시지가 기록됨
- 커밋은 가리키던 HEAD의 Child가 되며, 현재 브랜치의 가장 위에 기록됨
- 현재 가리키던 브랜치의 HEAD는 커밋이 수행된 인덱스를 가리키게 됨
- commit 이후에 실수가 발견되면 reset으로 복구가능 (추후 reset 파트에서 설명)

<aside>
💡 커밋을 할때 어떤 단위로 커밋을 해야 할까? 커밋은 일종의 작업 단위별 기록이기 때문에, **특정 작업이 종료되었을때 단위 단위로 커밋을 진행**하는것이 좋음. 또한 커밋 단위별로 해당 작업에 대한 변경점만 포함이 되어야 하지, 추가로 다른 것들도 같이 하거나 하는 것은 좋지 못함

</aside>

### rm | mv

- rm
  - 인덱스(.git)와 working tree 모두에서 파일을 제거
- 인덱스와 working tree에서 매치하는 패턴의 파일들을 삭제함
- git에서 제공하는 명령어로 working tree에서만 삭제하는것은 제공하지 않고, 해당 기능이 필요할 경우에는 `/bin/rm` 을 이용하도록 권장하고 있음 (os레벨 명령어)
- rm을 이용해서 삭제할 경우, working tree에서 삭제함과 동시에 `.git` 에서도 삭제한 상태로 commit을 기다리게 됨
- mv
  - 인덱스(.git)와 working tree 모두에서 파일의 이름을 변경
  - 이하의 내용은 rm과 동일

```bash
git rm [-f | --force] [-n] [-r] [--cached] [--ignore-unmatch]
	  [--quiet] [--pathspec-from-file=<file> [--pathspec-file-nul]]
	  [--] [<pathspec>…]

git mv <options>… <args>…
```

### log

- commit 이력을 보여줌

```bash
git log [<options>] [<revision-range>] [[--] <path>…]
```

- 대상 커밋과 커밋으로부터 따라갈수 있는 부모들의 커밋 집합을 표시
- ^를 대상에 붙일 경우 해당 커밋을 포함한 부모 커밋 집합은 결과에서 제외됨
- 예를 들어
  ```bash
  git log foo bar ^baz
  ```
  - 상기 명령어의 의미는 foo 커밋과 부모 커밋그룹, bar 커밋과 부모 커밋그룹을 표시하되, baz 커밋과 그 부모그룹은 결과에서 제외함을 의미
- 또한 이러한 명령도 가능
  ```bash
  git log origin..HEAD
  ```
  - 이는 origin 커밋과 HEAD (현재 포인터가 가리키는 인덱스) 사이의 커밋들 집합을 반환하라는 뜻
- 추가로 알아야 할 log 관련 트릭
  ```bash
  # 최근 커밋한 3개만 표시
  git log -3

  # 커밋 작성자로 필터링
  git log --author="author name"

  # 지정된 날짜 이전의 커밋을 필터링
  git log --before="2020-02-02"

  # 타이틀로 필터링
  git log --grep="title name"

  # 소스코드 안의 문자열로 필터링 (p 옵션은 patch로서 소스코드를 같이 표시)
  git log -S "some string" -p

  # 특정 파일명으로 필터링
  git log filename
  ```

### show

- 특정 커밋의 내용을 확인
- 커밋의 내용과 더불어 파일도 직접 열어서 확인 가능
  ```bash
  git show HEAD:filename
  ```

### tag

- GPG로 사인된 태그 오브젝트를 생성, 조회, 삭제등이 가능
- 커밋에 사용자 지정 문자열을 달아둔다고 생각하면 되는데, 일반적으로는 semantic versioning 기법을 이용함
  - semantic versioning
    [Semantic Versioning 2.0.0](https://semver.org/)

```bash
git tag [-a | -s | -u <keyid>] [-f] [-m <msg> | -F <file>] [-e]
	<tagname> [<commit> | <object>]
git tag -d <tagname>…
git tag [-n[<num>]] -l [--contains <commit>] [--no-contains <commit>]
	[--points-at <object>] [--column[=<options>] | --no-column]
	[--create-reflog] [--sort=<key>] [--format=<format>]
	[--merged <commit>] [--no-merged <commit>] [<pattern>…]
git tag -v [--format=<format>] <tagname>…
```

- 대상 커밋에 태그를 부여하는데, `-d / -l / -v` 옵션은 각각 삭제, 리스팅, 검증이기 때문에 추가는 그 외 옵션을 활용 가능
- 태그를 지정할때는 존재하지 않는 태그명을 입력해야 하는데, `-f` 옵션을 이용하면 기존 태그명을 삭제하고 해당 태그명을 강제로 이용함
