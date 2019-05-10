# 리모트 브랜치

- 리모트 Refs : 리모트 저장소에 있는 포인터의 레퍼런스. 리모트 저장소에 있는 브랜치, 태그 등.
- `git ls-remot <리모트저장소 이름>` : 모든 리모트 Refs를르 조회한다.
- `git remote show <리모트저장소 이름>` : 모든 리모트 브랜치의 목록과 각 브랜치의 정보를 조회한다.

## 리모트 트래킹 브런치

- 형식 : <리모트저장소 이름>/<브랜치 이름>
- 리모트 브랜치를 추적하는 레퍼런스이자 브랜치이자 포인터?
- 로컬에 있지만 임의로 조작할 수 없고, 리모트 서버에 연결할 때마다 리모트 브랜치의 업데이트 내용에 따라 자동으로 갱신된다.
- 리모트 저장소에 마지막으로 연결했을 때, 브랜치가 무슨 커밋을 가리키고 있는지를 나타냄.

### 예시

> > Github에 어떤 리모트 저장소가 있고, 이 저장소를 클론하면 git은 클론한 저장소에 자동으로 `origin`이라는 이름을 붙인다.<sub>default 리모트 저장소 이름</sub> origin 은 저장소로부터 데이터를 모두 내려받고, `master` 브랜치를 가리키는 포인터를 만든다. 이때 이 `master` 브랜치는 리모트 저장소에서 내려받은 리모트 트래킹 브랜치 `origin/master` 를 가리킨다.<sub>받은 시점에서부터 시작</sub>
> > 이 다음, 같이 작업하는 다른 팀원이 push를 하는 바람에 리모트 저장소의 `master`의 위치가 바뀌어도 로컬에 이미 받아둔 리모트 트래킹 브랜치 `origin/master`는 아무런 통신도 하지 않았기 때문에 포인터의 위치가 바뀌지 않는다.

- `git fetch <리모트저장소 이름>` : 리모트 서버로부터 저장소 정보를 동기화 한다.

  1. 명령을 실행하면 리모트 저장소의 주소 정보를 찾은 뒤, 현재 로컬 저장소가 가지고 있지 않은 새 정보를 모두 내려받는다.
  2. 받은 데이터를 로컬 저장소에 업데이트한다.
  3. `origin/master`의 위치를 최신 커밋으로 이동시킨다.<sub>다른 사람이 push한 위치를 가리킴</sub>

  ![git fetch로 동기화시키기](https://git-scm.com/book/en/v2/images/remote-branches-3.png)

- 리모트 저장소를 여러 개 운영하는 상황을 가정하고, 프로젝트 안에 리모트 저장소를 하나 더 추가한다면, 리모트 트래킹 브랜치 또한 <추가한 리모트 저장소의 이름>/master 으로 생겨 추가한 리모트 저장소의 `master` 브랜치가 가리키는 커밋을 가리킨다.
- `fetch`를 통해 리모트 트래킹 브랜치를 내려받는다고 해서 로컬 저장소에서 수정할 수 있는 브랜치가 새로 늘어나는 것은 아니다. 그냥 읽기전용 포인터용 브랜치가 하나 더 생기는 것 뿐.
- 새로 내려받은 브랜치의 내용을 merge 하려면 `git merge origin/리모트브랜치명` 을 사용한다.
- 리모트 트래킹 브랜치에서 시작하는 새 브랜치를 만들려면 `git checkout -b <새 브랜치 이름> <리모트 트래킹 브랜치 이름>` 을 사용한다.

## 로컬 브랜치를 리모트 저장소로 Push

- 로컬 브랜치를 서버로 전송하려면 쓰기 권한이 있는 리모트 저장소에 Push 해야 한다.
- 로컬 저장소의 브랜치는 자동으로 리모트 저장소에 전송되지 않고, 명시적으로 Push 해 주어야 한다.
- 사용하기에 따라 로컬에서만 쓰는 비공개 브런치를 만들수도, 토픽 브랜치만 전송할 수도 있다.
- `git push <리모트저장소 이름> <브랜치 이름>` : 지정된 리모트 저장소에 지정한 브랜치를 전송한다.
  - git은으 전송한 브랜치 이름을 `refs/heads/브랜치이름:refs/heads/브랜치이름` 으로 확장시킨다. 로컬 브랜치를 서버로 push 하고 리모트 저장소의 브랜치로 업데이트 한다는 의미이다.
- `git push <리모트저장소 이름> <로컬 브랜치 이름>:<서버에서 사용할 브랜치이름>` 을 이용해 로컬에서 사용하는 브랜치 이름과 서버에 올리는 브랜치 이름을 다르게 설정할 수 있다.

## 브랜치 추적

- Tracking Branch : 리모트 브랜치와 직접적인 연결 고리가 있는 로컬 브랜치. 트래킹 브랜치에서 `pull` 이나 `push` 명령을 내리면 리모트 저장소로부터 데이터를 내려받아 연결된 리모트 브랜치와 자동으로 merge 된다. 트래킹 대상이 되는 브랜치는 **Upstream Branch**<sup>[Upstream Branch의 별명](#UpstreamBranch)</sup> 라고 한다.

### Tracking Branch 만들기

- `git checkout -b <local branch name> <remote storage>/<remote branch>` 혹은 `git checkout --track <remote storage>/<remote branch>` 명령으로 트래킹 브랜치를 만들 수 있다. 전자는 만들고자 하는 트래킹브랜치와 리모트 브랜치를 직접 설정하는 명령이고, 후자는 트래킹 하고싶은 리모트 브랜치만 지정하고 로컬의 트래킹 브랜치는 자동으로<sub>(같은 이름으로)</sub> 만든다.

```
$ git checkout --track origin/remotebranch
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'remotebranch'
```

- `git branch -u <remote storage>/<remote branch>` 혹은 `git branch --set upstream-to <remote storage>/<remote branch>` 명령은 이미 로컬에 있는 브랜치가 리모트 저장소의 특정 브랜치를 추적하게 한다.

### Tracking Branch의 확인

- `git brnach -vv` : 추적 브랜치가 어떻게 설정되어 있는지 확인. 로컬 브랜치의 목록과 함께 로컬 브랜치가 추적하고 있는 리모트 브랜치도 함께 보여주며, 로컬 브랜치가 앞서가는지 뒤쳐지는지도 간단하게 보여준다.<sub>마지막으로 서버에서 가져온(fetch) 시점을 바탕으로 보여준다.</sub>

  ```
  C:\test\git>git branch -vv
    branch01 8ff60d1 [origin/remotebranch: ahead 2, behind 1] ConflictTest-master
    branch02 a5488b6 hotfix
  * master   914fb80 [origin/master] add_content

  ```

  - ahead : 로컬 브랜치에서 리모트 서버의 브랜치로 보내지 않은 커밋의 개수를 나타낸다.

  - behind : 리모트 서버의 브랜치에서 로컬브랜치로 아직 merge 하지 않은 커밋의 개수를 나타낸다.

- `git fetch --all; git branch -vv` : 서버로부터 최신 데이터를 받아온 후에 추적상황을 확인한다.

## 리모트 브랜치에서 Pull 하기

- `git fetch` : 리모트 저장소(서버)에는 존재하지만, 로컬에는 아직 없는 데이터를 받아와서 저장한 뒤, 사용자가 merge 할 수 있도록 준비해둔다. 워킹 디렉토리의 파일 내용은 변경되지 않고 그대로다. <sub>`git pull` 은 `fetch` 와 `merge` 작업을 하는 명령어이다.</sub>

- 일반적으로 `fetch` 와 `merge` 명령을 명시적으로 사용하는 게 `pull` 으로 한번에 두 작업을 하는 것보다 낫다.

## 리모트 브랜치 삭제하기

- `git push <current branch name> --delete <remotebranch name>` : 서버에서 더 이상 필요하지 않은 리모트 브랜치 삭제하기. 가비지 컬렉터가 돌기 전에는 데이터가 사라지지 않기 때문에 의도치 않게 삭제한 경우 등에도 커밋했던 데이터를 살릴 수 있다.

---

<strong id="UpstreamBranch">Upstream Branch의 별명</strong> 추적 대상인 리모트 브랜치 이름을 `@{u}` 나 `@{upstream}` 으로 짧게 사용할 수 있다. 예를 들어, `git merge <remote storage>/<remote branch>` 와 `git merge @{u}` 는 같은 동작을 수행한다.
