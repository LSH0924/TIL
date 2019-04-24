# Git의 기초

## 다른 VCS들이 데이터를 다루는 방식

대부분의 VCS시스템은 각 파일의 변화를 시간순으로 정리한 파일들의 목록만을 관리한다. <sub>보통 델타 기반 버전관리 시스템이라고 한다.</sub>
<br/>
![델타 기반 버전관리](https://git-scm.com/book/en/v2/images/deltas.png)

## Git이 데이터를 다루는 방식

Git은 데이터를 파일 시스템 **스냅샷<sup>[1](#snapshot)</sup>의 스트림**으로 취급하고, 크기가 매우 작다. Git은 사용자가 데이터를 커밋하거나, 프로젝트의 상태를 저장하는 시점의 순간을 중요시한다. 저장시의 파일과 이전 버전의 파일이 다른 점이 없으면 Git은 성능을 위해 파일을 새롭게 저장하지 않고, 이전 버전의 파일이 위치한 링크만 저장한다.
![Git의 버전관리](https://git-scm.com/book/en/v2/images/snapshots.png)

## 로컬에서 거의 모든 명령을 실행 가능

Git이 관리하는 프로젝트의 모든 히스토리가 로컬 디스크에 저장되기 때문에, 거의 모든 명령을 빠른 속도로 처리할 수 있다. 히스토리를 찾을 때, 굳이 서버에 접근 후 예전 버전을 찾아올 필요가 없다는 것. 때문에 오프라인 상태이거나, VPN에 연결하지 못해도 작업을 한 뒤 커밋할 수 있다. <sub>commit과 push는 다름..</sub>

## 무결성

Git은 데이터를 저장하기 전에 항상 체크섬<sup>[2](#checksum)</sup>을 구하고, 그 체크섬으로 데이터를 관리한다. 때문에, Git의 체크섬을 이해할 수 있는 Git없이 파일이나 디렉토리를 변경할 수 없으며, 파일의 상태도 알 수 없고, 데이터를 잃어버릴 수도 없다.

Git은 파일의 내용이나 디렉토리 구조와 SHA-1 해시를 사용하여 40자 길이의 16진수 문자열로 된 체크섬을 만든다.
<br/>Git은 모든것을 해시로 식별하며, 파일을 저장할 때도 이름이 아닌 해당 파일의 해시로 저장한다.

## Git은 데이터를 추가할 뿐

Git으로 무슨 짓을 하든 Git의 저장소에는 데이터가 **추가**될 뿐이다. 되돌리거나, 데이터를 삭제할 방법이 없다. 커밋하지 않았을 경우는 변경사항을 잃어버릴 수도 있다. 하지만, 일단 스냅샷을 커밋하고 나면 커밋한 데이터를 잃어버리기 어렵다.

## Git의 파일 상태 관리

Git은 파일을 Committed, Modified, Staged 의 세 가지 상태로 관리한다.

- **Committed** : 데이터가 로컬 데이터베이스에 안전하게 저장된 상태
- **Modified** : 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 상태
- **Staged** : 현재 수정한 파일을 곳 커밋할 것 이라고 표시한 상태

이 세 가지 상태는 Git 프로젝트의 세 단계와 관련되어 있다.

### Git 프로젝트의 세가지 단계

![Git 프로젝트의 세가지 단계](https://git-scm.com/book/en/v2/images/areas.png)

- **.git 디렉토리** : Git이 프로젝트의 메타 데이터와 객체 데이터베이스를 저장하는 곳. Git의 핵심. 다른 컴퓨터에 있는 저장소를 클론 할 때 .git 디렉토리가 생성된다.
- **워킹 트리** : 프로젝트의 특정 버전을 체크아웃한 것. .git 디렉토리 안에 있는 압축된 데이터베이스에서 파일을 가져와 워킹트리를 만든다.
- **Staging Area** : .git 디렉토리 안에 존재한다. 곧 커밋한 파일에 대한 정보를 저장. Git 에서는 index 라고 부른다.

### Git이 하는 일

워킹트리에서 파일 수정(파일 상태 Modified) -> Staging Area에 파일을 넣어서 커밋할 스냅샷 생성(파일 상태 Staged) -> Staging Area에 있는 파일을 .git 디렉토리에 영구적인 스냅샷으로 저장(커밋 : 파일상태 Committed)

## Git 설치 후 환경설정

git config 라는 도구로 Git을 설치하고 난 후에 환경설정 내용을 확인하고 변경할 수 있다. Git은 이 설정에 따라 동작한다. Git이 사용하는 설정파일은 세 가지 이다.

- **/etc/gitconfig**<sup>[3](#config)</sup> : 시스템의 모든 사용자와 모든 저장소에 적용되는 설정. `git config --system` 으로 이 파일을 읽거나 수정할 수 있다. 이 파일은 시스템 전체의 설정파일이기 때문에 수정하려면 시스템 관리자 권한이 필요하다.
- **~/.gitconfig, ~/.config/git/config** : 특정 사용자(현재 환경설정을 한 사용자)에게만 적용되는 설정. `git config --global`으로 이 파일을 읽거나 수정할 수 있다. 특정 사용자의 모든 저장소 설정에 적용된다.
- **.git/config** : .git 디렉토리 소속. 특정 저장소 혹은 현재 작업중인 프로젝트에만 적용된다. 환경설정을 변경할 프로젝트의 .git 디렉토리로 이동 후, `--local` 옵션으로 이 파일을 사용하도록 지정할 수 있다.<sub>하지만 기본적으로 사용되도록 지정 되어 있다.</sub>

우선순위는 `.git/config -> ~/.gitconfig, ~/.config/git/config -> /etc/gitconfig` 이다.

### 사용자 정보

커밋할 때마다 사용할 사용자 정보(이름, 이메일 주소)를 설정한다. 입력한 정보로 한 번 커밋하면 변경할 수 없다.

```
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

`--global` 옵션을 사용하면 현재 환경설정하는 사용자가 관리하는 모든 프로젝트에서 공통으로 이용할 사용자정보를 설정하게 된다,

만약 프로젝트마다 다른 사용자 정보를 사용하고싶으면, `--global` 옵션을 빼고 설정하면 된다.
GUI도구들은 처음 설치할 때 이 설정을 묻는다.

### 편집기

Git에서 사용할 텍스트 편집기 고르기. 기본적으로 Git은 시스템의 기본 편집기를 사용한다.

다른 편집기를 쓰고싶으면 아래처럼 설정한다.

```
$ git config --global 사용하고싶은 에디터
```

마찬가지로 GUI도구들은 처음 설치할 때 이 설정을 묻는다.

### 설정 확인하기

- `git config --list` 명령으로 환경설정한 리스트를 출력한다.

  ```
  C:\Users\user>git config --list
  core.symlinks=false
  core.autocrlf=true
  core.fscache=true
  color.diff=auto
  color.status=auto
  color.branch=auto
  color.interactive=true
  help.format=html
  rebase.autosquash=true
  ...
  ```

  Git은 같은 키를 여러 파일(/etc/gitconfig 와 ~/.gitconfig 등)에서 읽어오기 때문에 키가 중복될 수 있다. 키가 중복되어 있을 경우, Git은 나중에 읽어들인 파일의 키값을 사용한다.

- `git config <key>` 명령으로 특정 키에 어떤 값이 들어있는지 확인할 수 있다.

  ```
  C:\Users\user>git config color.diff
  auto
  ```

- `git config --show-origin <key>` 명령으로, 키에 설정된 값이 어떤 파일로부터 설정된 값인지를 확인한다.

  ```
  C:\Users\user>git config --show-origin color.diff
  file:"C:\\ProgramData/Git/config"       auto
  ```

### 도움말 보기

```
$ git help <verb>
$ man git-<verb>

// 예시
$ git help config
// 를 입력하면 브라우저에 도움말이 있는 페이지를 출력한다.
$ git config
// 간단 도움말을 보려면 이쪽
```

# Git Alias

`git config` 명령어를 이용해 Alias를 지정할 수 있다.

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.unstage 'reset HEAD --'
$ git config --global alias.last 'log -1 HEAD'
```

---

<strong  id="snapshot">[스냅샷]</strong> 과거의 어느 한 시점에 존재했던 컴퓨터 파일과 디렉토리에 대한 이미지를 복사해 유지시킨 묶음이다. 메모리 바이트, 하드웨어 레지스터, 상태 표시기 등의 모든 내용을 포함해 그 시점의 메모리 상태를 저장한다. 시스템 고장시 복구를 위해 주기적으로 저장된다.

<strong id="checksum">[체크섬]</strong> 검사합. 오류 검출방식 중 하나이다. 네트워크를 통해 전송된 데이터의 값이 변경되지는 않았는지, 무결성을 검사하는 값을 지칭한다. 대부분 데이터의 입력, 전송 시 제대로 되었는지를 확인하기 위해 입력, 전송데이터의 맨 마지막에 앞서 보낸 모든 데이터를 다 합한 합계를 따로 보낸다. 데이터를 받아들이는 쪽에서는 받아온 데이터를 하나 씩 합산한 다음, 합산 결과와 마지막으로 들어온 체크섬을 비교해 착오가 있는지 검사한다.

<p id="config">/etc/gitconfig파일은 C:\ProgramData\Git\ 디렉토리 안에서 찾을 수 있다. 이 시스템 설정 파일의 경로는 `git config -f <file>` 명령으로 바꿀 수 있다.(관리자 권한 필요)</p>

---

### 출처

[ProGit](https://git-scm.com/book/ko/v2)
<br>
[스냅샷](https://guswnsla1223.tistory.com/13)
<br>
[네이버 지식백과 : 검사합](https://terms.naver.com/entry.nhn?docId=842282&cid=42346&categoryId=42346)
