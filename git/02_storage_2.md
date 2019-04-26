# Git 저장소 만들고 사용하기 2

## Staged 와 Unstaged 상태의 변경내용 확인

단순히 파일 내용이 변경됐다는 사실이 아니라, 어떤 내용이 추가되고 삭제되었는지 보고싶으면 `git diff` 를 사용한다. 이 명령어는 워킹트리와 Staging Area에 있는 파일을 비교해 수정됐지만 아직 staged 되지 않은 변경사항을 보여준다.

Staging Area에 넣은 파일의 변경 부분을 보고싶다면, `--staged`나 `--changed` 옵션을 사용한다. 이 옵션을 사용하면 커밋이 끝난 시점의 파일과 Staging Area에 들어간 시점의 파일을 비교한다.

```
C:\test\git>git diff Modified.txt
diff --git a/Modified.txt b/Modified.txt
index e69de29..e4858dd 100644
--- a/Modified.txt // 맨 처음 add 했던 시점의 파일(Staged)
+++ b/Modified.txt // 수정 후 아직 add 하지 않은 파일(Unstaged)
@@ -0,0 +1,2 @@
+Modified
+s
\ No newline at end of file
```

`git diff` 외에도 `git difftool` 을 사용하면 vimdiff 등의 툴을 이용해 변경사항을 더 직관적으로 확인할 수 있다.

## 변경사항 커밋하기

- `git commit` : staged 상태의 파일들을 로컬데이터베이스에 저장. 이 명령어를 실행하면 열리는 기본 편집기 안에 커밋 메시지를 지정할 수있다.
  - `-v` : diff 메시지 추가
  - `-m` : 기본 편집기를 열지 않고 옵션 뒤에 메시지를 인라인으로 간단히 지정할 수 있다
  - `-a` : 모든 Tracked 상태의 파일을 자동으로 Staging Area에 넣고 커밋한다. 일일히 `git add`로 더해주지 않아도 됨.

```:커밋후 출력메시지
C:\test\git>git commit -m "first commit"
[master (root-commit) 6dc345f] first commit // 마스터 브랜치에 커밋. 체크섬 6dc345f. 커밋 메시지.
 2 files changed, 1 insertion(+)           // 두 파일이 바뀌었고 하나의 파일이 추가됨
 create mode 100644 Modified.txt
 create mode 100644 README.txt
```

## 파일 삭제

- 이미 커밋한 파일을 삭제하려면, `git rm` 명령어로 Tracked 상태의 파일을 Staging Area 에서 제거한 후 커밋해야 한다. 이 명령은 워킹 디렉토리에 있는 파일도 삭제해버리기 때문에 사용에 주의할 것.

- 명령어를 사용하지 않고 워킹 디렉토리 안에 있는 파일만 삭제할 경우 삭제한 파일은 `Unstaged` 상태가 된다. 실수로 데이터를 삭제할 것을 방지하는 장치.

- `git rm --cached` 옵션은 Staging Area 에서만 제거하고, 워킹트리에는 파일을 그대로 남겨둔다. 해당 파일을 Untracked 상태로 만듦.

- 여러 개의 파일이나 디렉토리를 한꺼번에 삭제할 수도 있다.(file-glob 패턴 사용)

  ```
  $ git rm log/\*.log // log 디렉토리 안에 있는 .log 파일을 모두 삭제한다.
  $ git rm \*~       // ~으로 끝나는 모든 파일을 삭제한다.
  ```

## 파일 이름 변경

- `git mv <기존 파일 이름> <새 파일 이름>` : 파일 이름을 변경한다. 단축 명령어.
  ```:git mv는 아래 명령어를 수행한것과 똑같다
  $ mv README.md README
  $ git rm README.md
  $ git add README
  ```
- 굳이 파일 이름을 `git mv`로 변경해주지 않아도 된다. 이름을 변경한 후 `git rm`, `git add` 명령을 실행하면 됨.

## 커밋 히스토리 조회

- `git log` : 프로젝트의 커밋 히스토리를 최근 시간순으로 조회한다.

  ```
  C:\test\git>git log
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)  // 커밋의 SHA-1체크섬
  Author: lee <fj8688ed@aa.jp.fujitsu.com>  // 저자 이름, 저자 이메일
  Date:   Tue Apr 23 11:10:21 2019 +0900   // 커밋 날짜, 커밋 메시지

      first commit
  ```

### git log 의 옵션

- `-p, --patch` : 각 커밋의 `diff` 결과를 보여준다.

  ```
  C:\test\git>git log -p
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)
  Author: lee <fj8688ed@aa.jp.fujitsu.com>
  Date:   Tue Apr 23 11:10:21 2019 +0900

      first commit

  diff --git a/Modified.txt b/Modified.txt
  new file mode 100644
  index 0000000..e69de29
  diff --git a/README.txt b/README.txt
  new file mode 100644
  index 0000000..0721dd9
  --- /dev/null
  +++ b/README.txt
  @@ -0,0 +1 @@
  +README!! Changed
  \ No newline at end of file
  ```

- `-2` : 최근 한 두 개의 커밋결과만 보여준다.
- `--stat` : 각 커밋의 통계 정보를 보여준다. 어떤 파일이 수정됐는지, 얼마나 많은 파일이 변경됐는지, 또 얼아마 많은 라인을 추가하거나 삭제했는지 보여준다. 요약정보는 가장 아래에 보여준다.

  ```
  C:\test\git>git log --stat
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)
  Author: lee <fj8688ed@aa.jp.fujitsu.com>
  Date:   Tue Apr 23 11:10:21 2019 +0900

      first commit

  Modified.txt | 0
  README.txt   | 1 +
  2 files changed, 1 insertion(+)
  ```

- `--pretty` : 히스토리 내용을 보여줄 때, 기본 형식이 아닌 다른 형식 중에서 하나를 선택할 수 있다.

  - oneline : 각 커밋을 한 라인으로 보여준다. 여러 개의 커밋 내용을 확인하기 좋음
  - graph : 각각의 브랜치와 머지 히스토리를 보여주는 아스키 그래프를 출력한다.
  - short : 커밋 메시지, 저자, 저자 이메일, 체크섬 등의 간략한 정보만 보여주기
  - full : 커밋 메시지, 저자, 커밋한 사람, 체크섬
  - fuller : 커밋 메시지, 저자, 저자 시각, 커밋한 사람, 커밋날짜, 체크섬

  ```
  C:\test\git>git log --pretty=oneline
  6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master) first commit

  C:\test\git>git log --pretty=short
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)
  Author: lee <fj8688ed@aa.jp.fujitsu.com>

      first commit

  C:\test\git>git log --pretty=full
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)
  Author: lee <fj8688ed@aa.jp.fujitsu.com>
  Commit: lee <fj8688ed@aa.jp.fujitsu.com>

      first commit

  C:\test\git>git log --pretty=fuller
  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master)
  Author:     lee <fj8688ed@aa.jp.fujitsu.com>
  AuthorDate: Tue Apr 23 11:10:21 2019 +0900
  Commit:     lee <fj8688ed@aa.jp.fujitsu.com>
  CommitDate: Tue Apr 23 11:10:21 2019 +0900

      first commit
  ```

  - format : 사용자 정의 결과 출력. 결과를 다른 프로그램으로 파싱하고자 할 때 유용하다. 포맷을 원하는대로 정확하게 일치시킬 수 있기 때문에 git을 새 버전으로 바꿔도 결과 포멧이 바뀌지 않는다.

    - 옵션

      - %H : 커밋 해시
      - %h : 짧은 길이 커밋 해시
      - %T : 트리 해시
      - %t : 짧은 길이 트리 해시
      - %P : 부모 해시
      - %p : 짧은 길이 부모 해시
      - %an : 저자 이름
      - %ae : 저자 메일
      - %ad : 저자 시각
      - %ar : 저자 상대적 시각
      - %cn : 커밋한 사람의 이름
      - %ce : 커밋한 사람의 메일
      - %cd : 커밋한 사람의 시각
      - %cr : 커밋한 사람의 상대적 시각
      - %s : 요약

    ```
    C:\test\git>git log --pretty=format:"%h - %an, %ar : %s"
    6dc345f - lee, 5 hours ago : first commit
    ```

- `--shortstat` : `--stat` 명령의 결과 중, 수정한 파일과 추가된 라인. 삭제된 라인만 출력
- `--name-only` : 커밋 정보 중에서 수정된 파일의 목록만 출력
- `--name-status` : 수정된 파일의 목록과 함께 파일의 추가, 수정, 삭제여부를 출력
- `--abbrev-commit` : SHA-1체크섬 40자 중 일부만 출력
- `--relative-date` : 정확한 시간이 아닌 명령을 내린 시점으로부터의 상대적 시간을 출력. ex) 2weeks ago 등
- `--since`, `--until` : 주어진 시간을 기준으로 필터링.
- 그 외, 저자 지정<sub>`--author`</sub>, 커밋 메시지에서 키워드 검색<sub>`--grep`</sub>, 주어진 옵션의 조건과 전부 일치하는 히스토리 검색<sub>`--all-match`</sub>, **코드에서 추가, 제거된 내용 중 특정 텍스트가 포함되어있는 지 검색<sub>`-S`</sub>**, 머지 커밋 표시하지 않기<sub>`--no-merges`</sub> 등이 있다.
