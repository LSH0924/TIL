# Git Branch

- 코드를 통째로 복사하고 나서 원래 코드와는 상관 없이 독립적으로 개발을 진행할 수 있음.
- 매우 가벼운 브랜치는 Git의 특장점이다.<sub>다른 버전관리 도구는 브랜치가 필요하면 프로젝트를 통째로 복사해야 했다.</sub>
- Git의 브랜치는 어떤 한 커밋을 가리키는 40자의 SHA-1 체크섬 파일(줄바꿈 포함 41Byte)에 불과하기 때문에 만들기도 쉽고 지우기도 쉽다.

## 브랜치(Branch)란?

- Git은 데이터를 스냅샷으로 기록한다.
- `commit` 명령을 내릴 때, Git은 현재 Staging Area 에 있는 데이터의 **스냅샷에 대한 포인터**와 저자, 커밋 메시지 등의 **메타데이터**, 이전 커밋에 대한 포인터 등을 포함하는 **커밋 Object**<sub>현재 커밋이 무엇을 기준으로 바뀌었는지 알 수 있음.</sub>를 저장한다.

### 예제 : 파일을 3개 가지고 있는 디렉토리를 커밋해보자.

1. 파일을 `add` 시켜 staged 상태로 만들면 Git은 저장소에 파일을 저장하고(Blob<sup>[1](#blob)</sup>으로 저장) Staging Area 에 해당 파일의 체크섬을 저장한다.

2. `git commit` 으로 커밋하기. 커밋할 때, 루트 디렉토리와 각각 하위 디렉토리의 트리 개체를 체크섬과 함께 저장소에 저장한다. 그 다음 커밋 Object를 만들고 메타 데이터, 루트 디렉토리 트리 개체를 가리키는 포인터 정보를 저장한다.<sub>이 작업으로 인해 커밋한 시점의 스냅샷은 필요에 따라 언제든지 다시 만들 수 있다.</sub>
   <br/>
   각각의 파일에 대한 Blob 세 개가 들어있고<sub>맨 오른쪽 노란거 세개</sub>, 파일과 디렉토리 구조를 가지고있는 트리 개체<sub>가운데 초록색. 각각 파일 blob을 가리킴.</sub>, 메타 데이터와 루트 트리<sub>가운데 초록색</sub>를 가리키는 포인터를 가진 커밋 개체<sub>맨 왼쪽 회색</sub>가 들어 있다.
   ![커밋을 마친 Git 저장소](https://git-scm.com/book/en/v2/images/commit-and-tree.png "600x400")
   <br/>
   파일을 수정하고 다시 커밋하면 바로 직전 커밋<sub>parent</sub>이 무엇인지도 저장한다.
   ![이전 커밋정보 저장](https://git-scm.com/book/en/v2/images/commits-and-parents.png "600x400")

## 새 브런치 만들기

- `git branch <브랜치 이름>` : 지정된 이름을 가진 브랜치를 하나 생성한다. 생성된 브랜치는 마지막 커밋을 가리킨다. 하지만 브랜치를 새로 만들었을 뿐, 생성한 브랜치로 옮겨간 것은 아니기 때문에 HEAD는 아직 작업중이던 브랜치를 가리킨다.

  !(현재 작업중인 브랜치를 가리키는 HEAD)[https://git-scm.com/book/en/v2/images/head-to-master.png]

- `git log --decorate` : 어느 브랜치가 어떤 커밋을 가리키는지 확인할 수 있다.

  ```
  C:\test\git\commitTest>git log --decorate --oneline
  1f71ff2 (HEAD -> master) files  // master 브랜치가 가리키고 있는 커밋
  248e63c (file) addFile          // file 브랜치가 가리키고 있는 커밋
  704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
  ```

## 브랜치 이동하기

- `git checkout <브랜치 이름>` : 지정한 브랜치로 이동하기. HEAD 또한 지정한 브랜치를 가리키게 된다.

  ```
  C:\test\git\commitTest>git checkout file
  Deletion of directory 'commitTest' failed. Should I try again? (y/n) n
  Switched to branch 'file'

  C:\test\git\commitTest>git log --decorate --oneline
  248e63c (HEAD -> file) addFile                        // HEAD가 file 브랜치를 가리킴
  704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit

  C:\test\git\commitTest>git checkout master
  Switched to branch 'master'

  C:\test\git\commitTest>git log --decorate --oneline
  1f71ff2 (HEAD -> master) files                        // HEAD가 master 브랜치를 가리킴
  248e63c (file) addFile
  704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
  ```

- 이동한 브랜치에 커밋하기

  ```
  C:\test\git\commitTest>git add branch.txt

  C:\test\git\commitTest>git commit -m branchTest
  [file 03dcf82] branchTest
  1 file changed, 1 insertion(+)
  create mode 100644 commitTest/branch.txt

  C:\test\git\commitTest>git log --oneline
  03dcf82 (HEAD -> file) branchTest   // 브랜치에 새로운 커밋 생성
  248e63c addFile
  704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
  ```

  - file 브랜치는 새 커밋을 함으로써 앞으로 이동했지만, master 브랜치는 여전히 자신의 마지막 커밋을 가리키고 있다.

  ```
  C:\test\git\commitTest>git checkout master

  C:\test\git\commitTest>git log --oneline --decorate
  1f71ff2 (HEAD -> master) files     // master 브랜치의 마지막 커밋
  248e63c addFile
  704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
  ```

  - 각 브랜치에서 커밋한 작업 내용들은 각각의 브랜치 안에 독립적으로 존재한다.

    ![갈라진 브랜치](https://git-scm.com/book/en/v2/images/advance-master.png)

  - 각 브랜치의 작업 내용을 통합하고 싶을 경우는 따로 Merge 해 주어야 한다.

- 브랜치를 이동하면 워킹 디렉토리의 파일은 그 브랜치에서 가장 마지막으로 했던 작업 내용으로 변경된다. 파일 변경시 문제가 있어 브랜치를 이동시킬 수 없는 경우, Git은으 브랜치 이동 명령을 수행하지 않는다.

## 전체 브랜치 확인하기

- `git log --oneline --decorate --graph --all` : 현재 브랜치가 가리키고 있는 히스토리와, 어떻게 갈라져나왔는지를 보여줌.

  ```
  C:\test\git\commitTest>git log --oneline --decorate --graph --all
  * 03dcf82 (file) branchTest
  | * 1f71ff2 (HEAD -> master) files
  |/
  * 248e63c addFile
  * 704a8d6 (tag: v0.1tag, tag: rm, tag: remove) tagging
  * 6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit
  ```

---

<strong id="blob">[blob]</strong> binary large object. 그래픽, 음성, 텍스트, 바이너리 데이터의 배열 등을 DB에 저장하기 위한 바이트 연속체

---

### 출처

[Blob](https://terms.naver.com/entry.nhn?docId=1592087&cid=50372&categoryId=50372)
<br>
