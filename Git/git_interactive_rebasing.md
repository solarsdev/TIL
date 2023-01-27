## interactive rebasing

```bash
git rebase [-i | --interactive] [<options>] [--exec <cmd>]
	[--onto <newbase> | --keep-base] [<upstream> [<branch>]]
git rebase [-i | --interactive] [<options>] [--exec <cmd>] [--onto <newbase>]
	--root [<branch>]
git rebase (--continue | --skip | --abort | --quit | --edit-todo | --show-current-patch)
```

- 해당 커밋부터 전부 새롭게 바뀜 (git rebase -i)
- WIP이 가리키는 이전 해시코드를 변경 (git rebase target~1)
- pick (냅두기), drop (버리기), reword (변경), squash (합치기) 등등
