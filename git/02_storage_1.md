# Git 저장소 만들고 사용하기

Pro Git 2장.
저장소를 만들고 설정하는 방법, 파일을 추적(track)하거나 추적을 그만두는 방법, 변경 내용을 stage 하고 커밋하는 방법, 파일이나 파일 패턴을 무시하도록 Git을 설정하는 방법, 실수를 쉽고 빠르게 만회하는 방법, 프로젝트 히스토리를 조회하고 커밋을 비교하는 방법, 리모트 저장소에 Push 하고 Pull 하는 방법에 대해서 설명한다.

## Git 저장소 만들기

### 기존 디렉토리를 Git 저장소로 만들기

버전관리를 하지 않는 기존 프로젝트를 Git으로 관리하고싶을 때, 해당 프로젝트의 디렉토리로 이동해서 `git init` 명령을 실행한다.

- `git init` : `.git` 디렉토리를 생성한다. `.git` 안에는 저장소에 필요한 뼈대 파일(skeleton)이 들어있다. `.git` 디렉토리를 만들었다고 해서 프로젝트의 파일을 관리하기 시작한 것은 아니다.

- `git add <fileName>` : storage에 커밋할 파일을 추가한다.

- `git commit -m <커밋메시지>` : 로컬 데이터베이스에 현재 시점의 파일 정보를 커밋한다.

```
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

### 기존 저장소 Clone

- `git clone <url>`

  - 다른 프로젝트에 참여하거나, Git 저장소를 복사 하고싶을 때 사용. 프로젝트 히스토리를 전부 다 받아온다.
  - 프로젝트 이름으로 된 디렉토리를 만들고, 그 안에 .git 디렉토리를 만든다. 그 후, 저장소의 데이터를 모두 가져온 뒤 가장 최신 버전을 checkout 한다.
  - URL 뒤에 따로 디렉토리의 이름을 지정할 수도 있다.

  ```
  $ git clone https://github.com/LSH0924/TIL
  $ git clone https://github.com/LSH0924/TIL sane'sTIL
  ```

## 파일 수정 후 저장하기

### 파일의 라이프사이클

워킹 디렉토리 안의 파일은 크게 Tracked(관리대상), Untracked(관리하지 않음) 로 나뉜다.

- **Tracked File(관리대상)** : 이미 스냅샷에 포함되어있던 파일.(Git이 이미 알고있던 파일) 이 상태 안에서 다시 Unmodified, Modified, Sraged 상태로 구분한다. 처음 저장소를 clone하면 파일을 Checkout 하고나서 아무것도 하지 않은 상태이기 때문에, 모든 파일이 Tracked 이면서 UnModified 상태이다.

- **Untracked File(관리하지 않음)** : 워킹 디렉토리에 있는 파일 중 스냅샷에도, Staging Area에도 포함되지 않는 파일.

마지막 커밋 후, 어떤 파일을 수정하면 Git은 그 파일을 **Modified** 상태로 인식한다. 커밋하기 위해 수정판 파일을 staged 상태로 만들고, staged 상태의 파일을 커밋한다. Git이 관리하는 프로젝트 내부의 파일은 이러한 라이프 사이클을 반복한다.

![파일의 라이프사이클](https://git-scm.com/book/en/v2/images/lifecycle.png)

### 파일의 상태 확인하기

파일의 상태를 확인하고싶을 때는 `git status` 명령을 사용한다.

```
C:\test\git>git status
On branch master
// default 브랜치
No commits yet
// 아직 아무 커밋도 없음
nothing to commit (create/copy files and use "git add" to track)
// 커밋 예정인것도 아무것도 없음. (track중인 파일 없음)
```

아무 README 파일을 생성 후 다시보자.

```
C:\test\git>git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        README.txt
// 관리대상이 아닌 파일이 하나 추가됨
nothing added to commit but untracked files present (use "git add" to track)
```

### 파일 추적하기

Untracked상태의 파일을 추적하고, 관리대상으로 넣고싶을 때는 `git add <fileName>` 를 사용한다. 이 명령은 파일 또는 디렉토리의 경로를 argument로 받아, 디렉토리일 경우 그 안에 있는 모든 파일들까지 재귀적으로 추가한다.

```
C:\test\git>git add README.txt

C:\test\git>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.txt
// Untracked 상태였던 파일을 track 대상에 넣은 후 staged 상태로 변경.
```

### Modified 상태의 파일 Stage 하기

아까 추가했던 README.txt 파일을 다시 수정하고 상태를 확인한다.

```
C:\test\git>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.txt
// `git add` 명령을 내렸던 시점1의 파일
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.txt
// storage 에 올라간 상태의 파일을 수정한 상태.(시점2) 아직 `add` 되지 않음.
```

이제 `git add` 명령을 내린 뒤 상태를 확인한다.

```
C:\test\git>git add README.txt

C:\test\git>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   README.txt
// 원래 상태로 돌아온것 처럼 보이지만, 시점1의 파일이 아닌 시점2의 파일이 staged 되어있다.
```

### 파일 상태를 짤막하게 확인

`git status` 명령어로 출력되는 내용이 길다면, `-s` 또는 `--short` 옵션을 붙여 짧은 버전으로 볼 수 있다.

```
C:\test\git>git status --short
AM Modified.txt // 다음 커밋대상이고, 추가된 시점 이후에 다시 편집되었음.
A  README.txt // 다음 커밋 대상
?? File1.txt // Untracked. 관리대상 아님
```

### 파일 무시하기

로그 파일이나 빌드시스템이 자동으로 생성하는 파일의 경우, 굳이 Git이 관리할 필요가 없다. 관리할 필요가 없는 파일을 무시하려면 `.gitignore`<sup>[1](#gitignore)</sup> 파일을 만들고 그 안에 무시할 파일 패턴을 적는다. `.gitignore` 파일은 보통 처음에 만들어두는 것이 편하다. 그래야 커밋 하고싶지 않은 파일을 실수로 커밋해버리는 불상사를 막을 수 있다.

```:.gitignore 파일의 작성예시
$ cat .gitignore
*.[oa] // 확장자가 .o이거나 .a인 파일을 무시
*~     // ~로 끝나는 모든 파일 무시
```

이 외에도 log, tmp, pid 등의 디렉토리나, 자동으로 생성하는 문서 등도 추가할 수 있다.

- `.gitignore` 파일에 입력하는 패턴의 규칙
  - 아무것도 없는 라인, # 으로 시작하는 라인은 무시한다.
  - 표준 Glob 패턴<sup>[2](#glob)</sup>을 사용한다. 이는 프로젝트 전체에 적용됨.
  - / 로 시작하면 하위 디렉토리에는 적용되지 않는다.(현재 디렉토리에만 적용됨)
  - 디렉토리는 이름 뒤에 / 를 붙이는 것으로 표현한다.
  - ! 로 시작하는 패턴의 파일은 무시하지 않는다.

---

<strong id="gitignore">[gitignore]</strong> .gitignore 관리예제(https://github.com/github/gitignore)

<strong id="glob">[glob]</strong> 정규표현식을 단순하게 만든 것. 보통 쉘에서 많이 사용한다. \* 는 문자가 하나도 없거나, 하나 이상을 의미하고, [abc] 는 중괄호 안에 있는 문자 중 하나를 의미한다. ? 는 문자 하나를 의미하며, [0-9] 처럼 중괄호 안의 캐릭터 사이에 - 을 사용할 경우는, 그 캐릭터 사이에 있는 문자 중 하나를 의미한다. 또한, ** 를 사용해 디렉토리 안의 디렉토리까지 지정할 수 있다. 예시) a/**/z -> a/z 혹은 a/b/z 나 a/b/c/z
