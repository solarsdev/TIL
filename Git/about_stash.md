## Stash에 대해서

### stash를 사용해야 하는 경우

- Git에는 working tree, staging area, .git directory의 개념으로 버전관리를 하게 되는데, 이때 working tree에서 편집을 진행하던 중 다른 브랜치로 checkout하여 작업을 진행해야 할 경우가 있음 (테스팅, 내용 수정 등등)
- 기존의 working tree의 내용을 커밋한 뒤에 checkout이 가능하다면 좋겠지만, 작업 내용이 어중간할 경우 곤란한 상황에 처할수 있음
- 이런 경우에는 Git에서 제공하는 stash기능을 이용하면 되는데, stash는 modified이면서 tracked 상태인 파일과 staging area에 있는 파일들을 보관해두는 장소로서 아직 끝내지 않은 수정사항을 스택에 잠시 저장했다가 나중에 다시 적용할 수 있음 (적용할때 꼭 원래 브랜치에서 적용해야 하는것은 아님)
- stash를 다른 브랜치나 깨끗하지 않은 working tree에 적용할수 있다는것은 내용에 충돌이 발생할수 있다는것을 의미함
