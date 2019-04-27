# 되돌리기

## 실수한 커밋 수정하기

너무 일찍 커밋했거나 어떤 파일을 빼먹었을 때, 커밋 메시지를 잘못 적었을 때 등 완료한 커밋을 수정하고싶을 때 `--amend` 옵션을 사용해 이전 커밋에 덮어씌울 수 있다. 이 명령은 이전의 커밋을 완전히 새로 고쳐서 새 커밋으로 변경한다. 이전에 실수했던 커밋은 히스토리에 남지 않는다.

```
$ git commit --amend
```

## 파일을 Unstage 상태로 돌리기

```
C:\test\git>git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   File1.txt
        modified:   Modified.txt


C:\test\git>git reset HEAD File1.txt

C:\test\git>git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   Modified.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        File1.txt
```

`git reset` 명령어는 Staging Area 와 워킹트리를 직접 건들이기 때문에 신중하게 사용해야 한다.

## Modified 파일 되돌리기

로컬에서 수정한 파일을 최근 커밋한 버전으로 되돌리고 싶다면 `git checkout --` 을 사용한다. 하지만 수정된 파일에 이전 커밋을 덮어쓰는 방식이기 때문에 신중하게 사용해야 한다.
