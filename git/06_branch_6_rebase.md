# Rebase

한 브랜치에서 다른 브랜치로 합지는 방법은 `merge` 말고도 또 하나가 있다. **Rebase** 는 한쪽 브랜치의 커밋을 Patch 로 만든 뒤 다른 한쪽 브랜치에 적용시킨다. 둘 다 합쳐지는 관점에서 보면 별다를 게 없지만, Rebase 는 Merge 보다 더 깨끗한 히스토리를 만들 수 있다. 때문에 Rebase 보통 리모트 브랜치에 커밋을 깔끔하게 적용하고싶을 때 사용한다.

<br/><br/>

Rebase는 브랜치의 변경사항을 순서대로 다른 브랜치에 적용시키면서 합치는 작업이고, Merge 는 두 브랜치의 최종 결과만 가지고 합치는 작업이다.

## Rebase 의 기초

1. 합쳐지는 브랜치를 checkout 한다.

2. `git rebase <합치는 브랜치 이름>` : 두 브랜치가 나뉘기 전의 공통 커밋으로 이동하고 난 뒤, 공통 커밋부터 현재 checkout 중인 브랜치가 가리키는 커밋까지 diff 를 만들어 임시로 저장한다. 그 다음, Rebase 할 브랜치의 포인터를 합칠 브랜치가 가리키는 커밋을 가라키게 하고 나서 방금 저장해두었던 변경사항을 차례대로 저장한다.

![합쳐지는 브랜치의 변경사항을 합치는 브랜치에 적용시키는 과정](https://git-scm.com/book/en/v2/images/basic-rebase-3.png)

3. 합치는 브랜치를 Fast-forward 시킨다.

```
$ git checkout <합치는 브랜치 이름>
$ git merge <합쳐지는 브랜치 이름>
```

4. Rebase 후, 필요가 없어진 브랜치를 삭제해도 커밋 사항은 남아있으니 안심.

## Rebase 활용하기 : 예제

![토픽 브랜치에서 또 갈라져 나온 토픽 브랜치](https://git-scm.com/book/en/v2/images/interesting-rebase-1.png)

위와 같이 하나의 토픽 브랜치에서 또다시 토픽 브랜치가 갈라져c 나와 있는 상황을 가정해본다. server 브랜치는 테스트가 덜 됐고 client 브랜치는 테스트가 완료된 상황이다. server 브랜치와 아무 관계가 없는 c8, c9 커밋을 master 브랜치로 합치고싶을 때 `--onto` 옵션을 사용할 수 있다.

```
$ git rebase --onto master server client
```

이 명령은 master 브랜치로부터 server 와 client 브랜치의 공통 조상까지의 커밋을 client 브블브치칭치에 없앤 뒤, client 브랜치에서만 변경된 사항들의 patch 를 만들어 master 브랜치에서 client 브랜치를 기반으로 만들어 새로 적용한다.

![다른 토픽 브랜치에서 갈라져 나온 토픽 브랜치를 Rebase](https://git-scm.com/book/en/v2/images/interesting-rebase-2.png)

이 다음 master 브랜치를 Fast-forward 시키면 끝.

### 간단하게 Rebase 시키기

- `git rebase <basebranch> <topicbranch>` : 토픽 브랜치를 체크아웃한 뒤 베이스 브랜치에 Rebase 한다.

## Rebase 의 위험성 : 이미 공개 저장소에 Push 한 커밋은 Rebase 금지

이미 공개 서버에 Push 해서 다른 사람들과 함께 사용하고 있는 커밋은 Rebase 할 경우, 서버의 히스토리에는 강제로 Push 할 수 있다. 하지만 Rebas 이전의 커밋을 clone 해서 사용하고 있는 다른 사람들이 다시 커밋하면 (이전에 Rebase로 없앴던) 같은 내용의 커밋이 두 개 존재하기 때문에 히스토리가 매우 복잡해져버려 혼란을 야기한다.

<br/><br/>

push 하기 전 정리용 Rebase 나, 로컬에서 Rebase는 얼마든지 해도 괜찮다. 하지만 이미 공개되서 사람들이 사용하는 커밋을 Rebase 하면 반드시 문제가 생긴다는건 알아두도록 하자.

## 다시 Rebase 하기

위의 상황과 같이, 이미 공유하고 있는 커밋을 한 팀원이 Rebase 한 뒤 강제 push 했을 때, 서버에 있는 히스토리를 더럽히지 않을 방법이 있다.

- `git rebase <remoteStorage>/<remoteBranch>` 명령을 실행하면 Git 은 아래 순서대로 작업을 진행한다.

  1. 현재 체크아웃 되어있는 브랜치에 포함된 커밋을 찾는다.
  2. Merge 커밋을 솎아낸다.
  3. 남은 커밋들 중 덮어쓰지 않은 커밋들만 지정한 브랜치를 바탕으로 커밋을 쌓는다.

- `git pull --rebase` 명령으로도 Rebase 할 수도 있다.<sub>`git fetch` 와 `git rebase <remoteStorage>/<remoteBranch>` 를 직접 순서대로 실행해도 됨.</sub>
- `git pull` 명령을 내릴 때 기본적으로 `--rebase` 옵션이 적용되도록 `pull.rebase` 설정을 추가 할 수 있다.

  ```
  git config --global pull.rebase true
  ```
