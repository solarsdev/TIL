## reset

- 현재 HEAD가 가리키고 있는 특정 상태를 초기화

```bash
git reset [-q] [<tree-ish>] [--] <pathspec>…
git reset [-q] [--pathspec-from-file=<file> [--pathspec-file-nul]] [<tree-ish>]
git reset (--patch | -p) [<tree-ish>] [--] [<pathspec>…]
git reset [--soft | --mixed [-N] | --hard | --merge | --keep] [-q] [<commit>]
```

- 상기 명령어중 위 3개는 지정한 tree-ish로부터 인덱스로 정보를 가져옴
- 마지막 명령은 현재 설정된 HEAD에서 commit으로 인덱스와 워킹 트리를 초기화 모든 tree-ish와 commit은 HEAD가 디폴트로 설정됨

```bash
git reset [-q] [<tree-ish>] [--] <pathspec>…
git reset [-q] [--pathspec-from-file=<file> [--pathspec-file-nul]] [<tree-ish>]
```

- 상기 명령어는 pathspec에 해당하는 tree-ish들을 모두 리셋하게 됨
- reset의 의미는 add의 정확히 반대이며, git restore —staged와 동일함
