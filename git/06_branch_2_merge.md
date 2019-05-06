# Merge의 기초

## 브랜치의 기초 : 예제

1. master 브랜치의 상태

```
C:\test\git\commitTest>git log --oneline --decorate
1f71ff2 (HEAD -> master) files
248e63c addFile
704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
```

2. 이슈 처리용 새 브런치를 만들자. 브랜치 만들기 + checkout까지 하려면 `git checkout -b <브랜치 이름>` 을 사용한다.

```
C:\test\git\commitTest>git checkout -b iss53  // 새 브런치를 만든 후 이동
Switched to a new branch 'iss53'

C:\test\git\commitTest>git add Issue53.txt    // 이슈 처리한 새 파일 추가하기

C:\test\git\commitTest>git commit -a -m Issue53  // 커밋
[iss53 07e7b45] Issue53
1 file changed, 0 insertions(+), 0 deletions(-)
create mode 100644 commitTest/Issue53.txt

C:\test\git\commitTest>git log --oneline --decorate --all --graph
* 07e7b45 (HEAD -> iss53) Issue53  // 커밋하면 브랜치 iss53이 앞으로 나아감
* 1f71ff2 (master) files
| * 03dcf82 (file) branchTest
|/
* 248e63c addFile
* 704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
* 6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
```

3. Issue53을 해결하던 도중, 갑자기 해결해야 할 버그가 생겼다고 하자. 버그를 해결한 HotFix 에 Issue53 이 섞이지 않도록 하려면 관련 소스코드를 따로 저장하거나 복구할 필요 없이 `master` 브랜치로 돌아가서 HotFix용 새로운 브랜치를 생성하면 된다.

- 아직 커밋하지 않은 파일이 checkout 할 브랜치와 충돌이 나면 브랜치를 변경할 수 없기 때문에, 브랜치를 변경할 때는 워킹 디렉토리를 정리해야 한다.

```
C:\test\git\commitTest>git checkout master
Switched to branch 'master'

C:\test\git\commitTest>git checkout -b hotfix01
Switched to a new branch 'hotfix01'

C:\test\git\commitTest>git add hotfix.js

C:\test\git\commitTest>git commit -m hotfix
[hotfix01 a5488b6] hotfix
 1 file changed, 1 insertion(+)
 create mode 100644 commitTest/hotfix.js

C:\test\git\commitTest>git log --oneline --decorate --all --graph
* a5488b6 (HEAD -> hotfix01) hotfix  // 새로 만든 hotfix01 브랜치
| * 07e7b45 (iss53) Issue53
|/
* 1f71ff2 (master) files
| * 03dcf82 (file) branchTest
|/
* 248e63c addFile
* 704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
* 6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
```

### 브랜치 Merge 하기

4. hotfix01 브랜치를 master 브랜치에 합치려면 `git merge` 명령을 내린다.

- Fast-forward : A 브랜치가 B 브랜치를 Merge 할 때, B 브랜치가 A 브랜치보다 최신 커밋을 가리키고 있으면 A브랜치가 통합한 B브랜치의 가장 최신 커밋을 가리키는 것. A 브랜치의 커밋이 따로 없거나, rebase 작업을 할 때 수행된다.

```
C:\test\git\commitTest>git checkout master  // 합치는 주체가 되는 브랜치로 이동
Switched to branch 'master'

C:\test\git\commitTest>git merge hotfix01   // hotfix01 브랜치를 master 브랜치에 합친다.
Updating 1f71ff2..a5488b6
Fast-forward
 commitTest/hotfix.js | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 commitTest/hotfix.js

// 중간에 file 브랜치도 합쳤다.

C:\test\git\commitTest>git log --oneline --decorate --all --graph
*   9527958 (HEAD -> master) Merge branch 'file'
|\
| * 03dcf82 (file) branchTest
* | a5488b6 (hotfix01) hotfix
| | * 07e7b45 (iss53) Issue53
| |/
|/|
* | 1f71ff2 files
|/
* 248e63c addFile
* 704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
* 6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
```

### 쓸모없어진 브랜치 삭제하기

5. 작업이 끝난 hotfix01 브랜치를 삭제하고, 이전에 처리중이던 iss53 브랜치로 돌아가자.

```
C:\test\git\commitTest>git branch -d hotfix01 file  // 쓸모없는 hotfix01, file 브랜치를 삭제
Deleted branch hotfix01 (was a5488b6).
Deleted branch file (was 03dcf82).

C:\test\git\commitTest>git checkout iss53  // iss53 브랜치로 돌아가 작업 계속하기
Switched to branch 'iss53'
```

## Merge 의 기초 : 위의 예제에 이어서

6. 작업이 끝난 iss53브랜치를 master 브랜치와 merge 시키자.

```
C:\test\git\commitTest>git checkout master
Switched to branch 'master'

C:\test\git\commitTest>git merge iss53
Merge made by the 'recursive' strategy.
 commitTest/Issue53.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 commitTest/Issue53.txt
```

- 위의 hotfix01 브랜치를 master 브랜치에 합칠때와는 달리 Fast-forward 를 사용하지 않는다. 현재 브랜치가 가리키는 커밋이 Merge 할 브랜치의 조상이 아니기 때문. 이 경우 Git은 각각의 브랜치가 가리키는 커밋 두 개와 공통 조상 하나를 사용해 3-way merge<sup>[1](#3-way merge)</sup>를 한다.

---

<strong id="3-way merge">[3-way merge]</strong> 단순히 브랜치 포인터를 최신 커밋으로 옮기는 게 아닌, 각 브랜치의 commit을 merge한 후 결과를 별도 커밋<sub>merge commit</sub>으로 만들고 나서 해당 브랜치가 그 커밋을 가리키도록 이동시킨다. 양쪽 브랜치에 각각 commit 이 있으면 수행된다.
