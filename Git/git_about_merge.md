## Merge에 대해서

- merge는 git으로 버전관리를 수행함에 있어서 핵심을 관통하는 개념이기 때문에 자세하게 공부해야 할 필요가 있음
- merge의 2가지 방식에 대한 이해
  1. fast forward merge
  2. three way merge
- merge 진행시 충돌 해결 방법에 대한 이해

### fast-forward-merge

- master 브랜치에서 파생된 기능 브랜치의 작업이 종료된 후 병합하는 과정에서, master 브랜치에서 또 다른 파생 브랜치가 없다면 master의 포인터를 기능 브랜치로 옮겨주기만 하면 병합이 완료됨

```bash
# original

      A---B---C topic
     /
D---E master

# fast forward merge

      A---B---C master
     /
D---E
```

- 이러한 fast forward 병합의 경우 기존의 topic이라는 브랜치가 있었는지도 모르게 병합이 되는데, 이러한 방식을 선호하는 경우와 그렇지 않은 경우가 있음
- 기본적으로는 fast forward 방식으로 merge가 수행되지만, 명시적으로 fast forward를 수행하지 않도록 옵션을 줄 수 있는데 `—no-ff` 옵션을 주면 됨

```bash
# original

      A---B---C topic
     /
D---E master

# no fast forward (with --no-ff)

      A---B---C topic
     /        |
D---E---------F master (merge commit)
```

- 이때 F는 새로운 커밋으로 작성됨 (Merge branch ‘topic’)

### three-way-merge

- master 브랜치에서 직접 파생된 브랜치로 fast forward가 가능한데 비해, master 브랜치의 이전 커밋으로부터 파생된 브랜치를 master 브랜치로 merge하기 위해서는 three-way-merge 방식을 이용해야 함

```bash
# original

      A---B---C master
     /
D---E---F feature

# three way merge

      A---B---C---G master (merge commit)
     /           /
D---E---F-------- feature
```

- G는 merge를 하기 위한 전용 commit임에 주목
- 왜 three way merge를 수행하는것이 좋은걸까?
  - 비교를 위해 필요한 3개의 커밋을 정리하면 다음과 같음
    ![git_about_merge/1.png](git_about_merge/1.png)
    1. 현재 브랜치의 커밋
    2. 병합 대상 브랜치의 커밋
    3. 두 브랜치의 공통 조상이 되는 커밋 (base)
- 공통 조상인 Base에 커밋되어 변경된 부분이 `a,b,c,d`라고 가정하고 다음 표로 관리한다고 생각해보자
  ![git_about_merge/2.png](git_about_merge/2.png)
- 여기서 현재 브랜치 (My)와 병합 대상 브랜치 (Other)에서 변경된 내역은 각각 아래의 표와 같다
  ![git_about_merge/3.png](git_about_merge/3.png)
- 첫번째 a를 보게 되면 현재 브랜치에서는 변경된것이 없고 병합 대상 브랜치에서만 변경이 일어났다. 두번째 브랜치에서는 양쪽 다 변경이 없으며, c는 둘다 변경이 일어났다. d는 현재 브랜치에서만 변경이 일어났음을 알 수 있다.
- 이때 Base가 없다고 가정하면 다음과 같은 일이 발생함
  ![git_about_merge/4.png](git_about_merge/4.png)
- 2 way merge의 경우에는 양쪽에서 동일한 코드로 보이는 b를 제외하면 어떤 코드를 취해야 하는지 알 수 없으므로 충돌로 판정이 될 수밖에 없음
- 그러나 공통 조상을 포함하여 3 way merge를 이용할 경우 자동으로 어떤 커밋을 취해야 할지 그 자리에서 결정할 수 있음
  ![git_about_merge/5.png](git_about_merge/5.png)
