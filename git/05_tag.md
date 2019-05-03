# 태그(Tag)

릴리즈할 때 사용.

## 태그 조회

- `git tag` : 이미 만들어진 태그가 있는 지 확인한다. 알파벳 순서대로 태그를 보여준다. 검색 패턴을 사용해 태그를 검색할 수 있다.

  - `--list` : 혹은 `-l`. 암묵적으로 적용됨? 와일드 카드를 이용해 태그 리스트를 확인하고자 할 때 반드시 사용해주어야 한다.

## 태그 설정

- Annotated Tag : Git 데이터베이스에 태그를 만든 사람의 읾, 이메일, 태깅 날짜, 태그 메시지를 저장한다. GPC(GNU Privacy Guard) 로 서명할 수도 있다. 일반적으로는 Annotated Tag 를 만들어 이 모든 정보를 사용할 수 있도록 하는것이 좋음.

  - `git tag -a <태그이름> -m <태그 메시지>` 명령으로 Annotated Tag 를 만든다. 명령을 실행할 때 옵션을 따로 설정하지 않으면, Git은 편집기를 실행시킨다.

  ```
  C:\test\git>git tag -a v0.0 -m "First Tag"

  C:\test\git>git tag
  v0.0

  C:\test\git>git show v0.0  // git show 명령어로 태그 정보와 커밋 정보를 모두 확인할 수 있다.
  tag v0.0
  Tagger: lee <fj8688ed@aa.jp.fujitsu.com>
  Date:   Wed Apr 24 15:27:23 2019 +0900

  First Tag

  commit 6dc345fc64a15e1b4576219142cf9601fd3a5789 (HEAD -> master, tag: v0.0, tag: show)
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

- LightWeight Tag : 파일에 커밋 체크섬을 저장한다. 브랜치와 비슷하지만. 브랜치처럼 가리키는 지점을 최신 커밋으로 이동시킬 수는 없다. 단순히 특정 커밋에 대한 포인터로 작용함.

  - 다른 옵션은 사용하지 않고 이름만 달아준다.

  ```
  C:\test\git>git tag v0.1

  C:\test\git>git tag
  show
  v0.0
  v0.1
  ```

## 예전 커밋에 태그달기

특정 커밋에 태그 할 수 있다. 태그 명령어 끝에 커밋의 체크섬을 명기한다. 이때, 체크섬을 전부 적을 필요는 없다.

```:예시가 이상하니까 다시 하자
C:\test\git>git log --oneline
704a8d6 (HEAD -> master) tagging  //이 커밋에 태그를 붙여보자
6dc345f (tag: v0.1, tag: v0.0, tag: show) first commit

C:\test\git>git tag v0.1tag 704a8d6  // 방금 커밋의 체크섬 추가

C:\test\git>git tag
show
v0.0
v0.1
v0.1tag  // 태그 생성됨

C:\test\git>git show v0.1tag
commit 704a8d6731d1379166bbe42580c6bba782de6354 (HEAD -> master, tag: v0.1tag)
Author: lee <fj8688ed@aa.jp.fujitsu.com>
Date:   Wed Apr 24 15:39:50 2019 +0900

    tagging

diff --git a/Modified.txt b/Modified.txt
index e69de29..e4858dd 100644
--- a/Modified.txt
+++ b/Modified.txt
@@ -0,0 +1,2 @@
+Modified
+s
\ No newline at end of file
```

## 태그 공유

- `git push <리모트 저장소 이름> <태그 이름>` : 일반적인 push로는 태그를 리모트 서버에 전송할 수 없으므로, 따로 태그만 push 해주어야 한다.

  - `--tags` : 이 옵션을 추가하면 모든 태그를 리모트 서버에 전송할 수 있다.

## 태그의 체크아웃

태그를 사용해 특정 버전의 파일들을 체크아웃 하고싶을 때는 아래 명령을 사용한다.

```
$ git checkout <버전의 태그>
```

단, 브랜치로 체크아웃 하는 게 아닌 태그로 체크아웃 하는 경우 "detached HEAD"<sup>[1](#detached_HEAD)<sup> 상태가 되며, 일부 git 관련 작업이 브랜치에서 작업하는거랑은 다르게 동작할 수 있음. 때문에 버그픽스 등의 의미있는 작업을 할 경우는 반드시 브랜치를 만들어야 한다.

---

<strong id="detached_HEAD">[detached HEAD]</strong> 체크아웃한 시점의 파일을 둘러보고, 자유롭게 커밋할 수 있으나 로컬 저장소에 push 하는 것은 지원되지 않는다. 다른 시점을 checkout 할 경우, 지금까지 한 모든 실험적 커밋을 버린다. 만약 커밋한 내용을 적용하고 싶다면 브랜치를 새로 만드는게 좋다.

---

## 출처

[Git の 'detached HEAD' 状態とその注意点 ](https://yu8mada.com/2018/05/31/detached-head-state-and-its-caution-in-git/)
