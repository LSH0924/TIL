# 브랜치 관리

- `git branch` : 아무런 옵션 없이 실행하면 모든 브랜치 목록을 보여준다.

  ```
  C:\test\git\commitTest>git branch
    branch01
    branch02
  * master     // 브랜치이름 옆의 아스타리스크는 현재 체크아웃중인 브랜치를 나타낸다.
    nomergedBranch
  ```

- `-v` : 모든 브랜치 목록과 함께 마지막 커밋 메시지도 출력

  ```
  C:\test\git\commitTest>git branch -v
    branch01       8ff60d1 ConflictTest-master
    branch02       a5488b6 hotfix
  * master         914fb80 add_content
    nomergedBranch 3028122 issue53
  ```

- `--merged`, `--no-merged` : 현재 체크아웃 한 브랜치를 기준으로 Merge된 브랜치인지 아닌지 필터링한다. 옵션 뒤에 특정 브랜치 이름을 넣어 지정한 브랜치를 기준으로 하는 목록을 출력할 수도 있다.

  ```
  C:\test\git\commitTest>git branch --merged   // Merge 작업을 수행한 브랜치. 또는 merge 할 별도의 커밋이 없는 브랜치
    branch01
    branch02
  * master

  C:\test\git\commitTest>git branch --no-merge  // 아직 Merge 할 커밋이 남아있는 브랜치
    nomergedBranch
  ```

  - 아직 merge 할 커밋이 남아 있는 브랜치는 일반적으로는 삭제할 수 없다. 강제로 삭제하려면 `-D` 옵션으로 삭제한다.

  ```
  C:\test\git\commitTest>git branch -d nomergedBranch
  error: The branch 'nomergedBranch' is not fully merged.
  If you are sure you want to delete it, run 'git branch -D nomergedBranch'.

  C:\test\git\commitTest>git branch -D nomergedBranch
  Deleted branch nomergedBranch (was 3028122).
  ```

# 브랜치의 워크플로

## Long-Running Branch

- 브랜치를 이용해 여러 단계에 걸쳐 안정화시켜 나가면서 충분히 안정화 됐을 때 안정화 브랜치로 Merge.
- 배포했거나, 배포할 코드<sub>(테스트를 거친 안정적인 코드)</sub>만 `master`브랜치에 merge 해둔다.
- 개발용, 안정화용 브랜치는 `develop`, `next` 등의 이름으로 추가해 사용한다.
- 개발용, 안정화용 브랜치는 공격적으로 히스토리를 만들며 나아가고, 배포용 브랜치는 그 히스토리를 따라간다.

![Long-Running-Branch](https://git-scm.com/book/en/v2/images/lr-branches-1.png)

- 프로젝트 규모가 크고 복잡할 수록 유용하다.
- 코드를 여러 단계로 나누어 안정성을 높여가며 운영할 수 있다.
- 해당 토픽을 해결하고 난 후, 테스트를 거쳐 안정성이 증명되면 merge하는 식으로, 토픽 브랜치에도 적용시킬 수 있다.

## 토픽 브랜치

- 어떤 한 가지 주제나 작업을 위해 만든 짧은 호흡의 브랜치.
- 주제별로 브랜치를 만들고, 각각의 브랜치가 독립 되어있기 때문에 묶음별로 나눠서 일하면서 내용별로 검토, 테스트 하기 편하다.
- 각자 작업을 유지하다가 토픽이 끝나면 `master`에 merge한다.
- 프로젝트 크기에 상관없이 유용하다움
- 한 가지 이슈를 처리하기 위해 여러 가지 방법을 고안해볼 수 있다. 물론, 각각의 브랜치는 서로에게 영향을 주지 않는다.
  ![하나의 이슈를 다양한 방법으로 처리하기 쉬움](https://git-scm.com/book/en/v2/images/topic-branches-1.png)
