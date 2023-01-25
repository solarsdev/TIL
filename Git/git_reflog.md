## reflog

- 로그 참조를 관리

```bash
git reflog [show] [<log-options>] [<ref>]
git reflog expire [--expire=<time>] [--expire-unreachable=<time>]
	[--rewrite] [--updateref] [--stale-fix]
	[--dry-run | -n] [--verbose] [--all [--single-worktree] | <refs>…]
git reflog delete [--rewrite] [--updateref]
	[--dry-run | -n] [--verbose] <ref>@{<specifier>}…
git reflog exists <ref>
```

- reflog에서 참조하는 정보는 커밋 기반이기 때문에, 커밋을 했다면 해당 로그를 기반으로 reset —hard를 통해서 돌아갈 수 있음
