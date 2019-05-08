# 충돌(confilct)의 기초

가끔 3-way merge가 실패할 때가 있다. Merge 하려고 하는 두 브랜치에서 같은 파일의 같은 부분을 동시에 수정했을 때, git은 해당 부분을 merge하지 못하고 confilct 메시지를 출력한다.

## 충돌 예제

1.아래 내용의 tracked 된 confilctEx.txt 파일이 있다고 하자.

> > Confilct Example
> >
> > MasterBranch

2.새 브랜치를 만들어서 confilctEx.txt를 수정하고 커밋한다.

- 수정한 내용
  > > Conflict Example
  > >
  > > ConflictBr01 Branch

```
C:\test\git\commitTest>git checkout -b conflictBr01
Switched to a new branch 'conflictBr01'

C:\test\git\commitTest>git commit -a -m ConflictTest-conflictBr01
[conflictBr01 b4e76d3] ConflictTest-conflictBr01
1 file changed, 2 insertions(+), 2 deletions(-)
```

3. master 브랜치로 이동 후, 다시 새 브랜치를 만들어서 confilctEx.txt의 같은 부분을 수정하고 커밋한다.

- 수정한 내용
  > > Conflict Example
  > >
  > > This File Will be Conflict. Branch

```
C:\test\git\commitTest>git checkout master
Switched to branch 'master'

C:\test\git\commitTest>git checkout -b conflictBr02
Switched to a new branch 'conflictBr02'

C:\test\git\commitTest>git commit -a -m ConflictTest-conflictBr02
[conflictBr02 d5f1151] ConflictTest-conflictBr02
 1 file changed, 1 insertion(+), 1 deletion(-)
```

4. 현재 상황을 그래프로 보자면 이렇다.

```
C:\test\git\commitTest>git log --oneline --decorate --all --graph
* d5f1151 (conflictBr02) ConflictTest-conflictBr02
| * b4e76d3 (conflictBr01) ConflictTest-conflictBr01
|/
* 8ff60d1 (HEAD -> master) ConflictTest-master

...(아래 생략)...
```

이제 브랜치 conflictBr01과 conflictBr02를 merge 해보자.

```
C:\test\git\commitTest>git checkout conflictBr01
Switched to branch 'conflictBr01'

C:\test\git\commitTest>git merge conflictBr02   // merge 실패. conflict 메시지를 출력한다.
Auto-merging commitTest/conflictEx.txt
CONFLICT (content): Merge conflict in commitTest/conflictEx.txt
Automatic merge failed; fix conflicts and then commit the result.
```

충돌난 파일을 살펴보자.

```
C:\test\git\commitTest>git status
On branch conflictBr01
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   conflictEx.txt       // 얘가 충돌남.

no changes added to commit (use "git add" and/or "git commit -a")
```

> > Conflict Example

> > <<<<<<< HEAD
> > ConflictBr01 Branch // 현재 HEAD가 위치한 버전의 파일상태
> > =======
> > This File Will be Conflict. Branch // merge하려던 브랜치의 파일상태
> >
> > > > > > > > > conflictBr02

충돌을 해결하려면 위나 아래쪽 내용에서 고르거나, 새로 작성해서 merge한 뒤 다시 `git add` 명령을 내려 저장한다. 또는, `git mergetool` 을 실행해 고칠 수 있다. `mergetool` 로 수정이 완료되면 자동으로 `git add` 가 수행되어 파일이 저장된다.<br/>
파일의 내용을 새로 작성한 뒤 커밋했다.

> > Conflict Example
> >
> > Conflict Test The End

```
C:\test\git\commitTest>git add conflictEx.txt

C:\test\git\commitTest>git status
On branch conflictBr01
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

        modified:   conflictEx.txt

C:\test\git\commitTest>git commit -a -m merge!!  // 수정한 내용은 커밋과 함께 merge 된다.
[conflictBr01 074001e] merge!!
```
