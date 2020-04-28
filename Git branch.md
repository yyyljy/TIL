# Git branch

## 1. branch 관련 명령어

> Git 브랜치를 위해 root-commit을 발생시키고 진행하세요.

1. 브랜치 생성

   ```
   (master) $ git branch {브랜치명}
   ```

2. 브랜치 이동

   ```
   (master) $ git checkout {브랜치명}
   ```

3. 브랜치 생성 및 이동

   ```
   (master) $ git checkout -b {브랜치명}
   ```

4. 브랜치 삭제

   ```
   (master) $ git branch -d {브랜치명}
   ```

5. 브랜치 목록

   ```
   (master) $ git branch
   ```

6. 브랜치 병합

   ```
   (master) $ git merge {브랜치명}
   ```

   - master 브랜치에서 {브랜치명}을 병합

## 2. branch 병합 시나리오

> branch 관련된 명령어는 간단하다.
>
> 다양한 시나리오 속에서 어떤 상황인지 파악하고 자유롭게 활용할 수 있어야 한다.

### 상황 1. fast-foward

> fast-foward는 feature 브랜치 생성된 이후 master 브랜치에 변경 사항이 없는 상황

1. feature/test branch 생성 및 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git checkout -b feature/test
   Switched to a new branch 'feature/test'
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   ```

2. 작업 완료 후 commit

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   $ touch test.txt
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   $ git add .
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   $ git commit -m 'Complete'
   [feature/test 72c67c4] Complete
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   ```

3. master 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/test)
   $ git checkout master
   Switched to branch 'master'
   
   student@M15016 MINGW64 ~/Desktop/branch (master)
   ```

4. master에 병합

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git merge feature/test
   Updating 7e6c08f..72c67c4
   Fast-forward
    test.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 test.txt
   
   student@M15016 MINGW64 ~/Desktop/branch (master)
   ```

5. 결과 -> fast-foward (단순히 HEAD를 이동)

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git log --oneline
   72c67c4 (HEAD -> master, feature/test) Complete
   7e6c08f Complete menu
   b056111 Init
   ```

6. branch 삭제

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git branch -d feature/test
   Deleted branch feature/test (was 72c67c4).
   ```

------

### 상황 2. merge commit

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 **다른 파일이 수정**되어 있는 상황
>
> git이 auto merging을 진행하고, **commit이 발생된다.**

1. feature/signout branch 생성 및 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git checkout -b feature/signout
   Switched to a new branch 'feature/signout'
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/signout)
   ```

2. 작업 완료 후 commit

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/signout)
   $ touch signout.txt
   $ git add .
   $ git commit -m 'Complete add'
   [feature/signout 87bcbd2] Complete add
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.txt
   
   ```

3. master 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/signout)
   $ git checkout master
   Switched to branch 'master'
   ```

4. *master에 추가 commit 이 발생시키기!!*

   - **다른 파일을 수정 혹은 생성하세요!**

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ touch hotfix.txt
   $ git add .
   $ git commit -m 'hotfix'
   [master 5a6c6df] hotfix
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 hotfix.txt
   ```

5. master에 병합

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git merge feature/signout
   Merge made by the 'recursive' strategy.
    signout.txt | 0
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 signout.txt
   ```

6. 결과 -> 자동으로 *merge commit 발생*

   - vim 편집기 화면이 나타납니다.

   ![image-20200424104547372](C:\iot\Git branch\image-20200424104547372.png)

   - 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.
     - `w` : write
     - `q` : quit

- 커밋이 확인 해봅시다.

  ```bash
  student@M15016 MINGW64 ~/Desktop/branch (master)
  $ git log --oneline
  92f15f2 (HEAD -> master) Merge branch 'feature/signout'
  5a6c6df hotfix
  87bcbd2 (feature/signout) Complete add
  72c67c4 Complete
  7e6c08f Complete menu
  b056111 Init
  ```

  ```bash
  student@M15016 MINGW64 ~/Desktop/branch (feature/signout)
  $ git log --oneline
  87bcbd2 (HEAD -> feature/signout) Complete add
  72c67c4 Complete
  7e6c08f Complete menu
  b056111 Init
  ```

1. 그래프 확인하기

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git log --oneline --graph
   *   92f15f2 (HEAD -> master) Merge branch 'feature/signout'
   |\
   | * 87bcbd2 (feature/signout) Complete add
   * | 5a6c6df hotfix
   |/
   * 72c67c4 Complete
   * 7e6c08f Complete menu
   * b056111 Init
   ```

2. branch 삭제

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git branch -d feature/signout
   Deleted branch feature/signout (was 87bcbd2).
   ```

------

### 상황 3. merge commit 충돌

> 서로 다른 이력(commit)을 병합(merge)하는 과정에서 **같은 파일의 동일한 부분이 수정**되어 있는 상황
>
> git이 auto merging을 하지 못하고, 충돌 메시지가 뜬다.
>
> 해당 파일의 위치에 표준형식에 따라 표시 해준다.
>
> 원하는 형태의 코드로 직접 수정을 하고 직접 commit을 발생 시켜야 한다.

1. feature/board branch 생성 및 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git checkout -b feature/board
   Switched to a new branch 'feature/board'
   
   student@M15016 MINGW64 ~/Desktop/branch (feature/board)
   ```

2. 작업 완료 후 commit

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/board)
   $ touch board.txt
   $ git add .
   $ git commit -m 'board complete'
   [feature/board 72514e3] board complete
    2 files changed, 1 insertion(+)
    create mode 100644 board.txt
   ```

3. master 이동

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (feature/board)
   $ git checkout master
   Switched to branch 'master'
   
   student@M15016 MINGW64 ~/Desktop/branch (master)
   ```

4. *master에 추가 commit 이 발생시키기!!*

   - **동일 파일을 수정 혹은 생성하세요!**

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ touch README.md
   $ git add .
   $ git commit -m 'master README.md'
   [master 0861a4f] master README.md
    1 file changed, 2 insertions(+)
   ```

5. master에 병합

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git merge feature/board
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```

6. 결과 -> *merge conflict발생*

   > git status 명령어로 충돌 파일을 확인할 수 있음.

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master|MERGING)
   $ git status
   On branch master
   You have unmerged paths.
     (fix conflicts and run "git commit")
     (use "git merge --abort" to abort the merge)
   
   Changes to be committed:
           new file:   board.txt
   
   Unmerged paths:
     (use "git add <file>..." to mark resolution)
           both modified:   README.md
   ```

7. 충돌 확인 및 해결

   

8. merge commit 진행

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master|MERGING)
   $ git add .
   $ git commit
   [master 5558c76] Merge branch 'feature/board'
   ```

   - vim 편집기 화면이 나타납니다.

     ![image-20200424111758644](C:\iot\img\Git branch\image-20200424111758644.png)

   - 자동으로 작성된 커밋 메시지를 확인하고, `esc`를 누른 후 `:wq`를 입력하여 저장 및 종료를 합니다.

     - `w` : write
     - `q` : quit

   - 커밋이 확인 해봅시다.

9. 그래프 확인하기

   ```bash
   student@M15016 MINGW64 ~/Desktop/branch (master)
   $ git log --oneline --graph
   *   5558c76 (HEAD -> master) Merge branch 'feature/board'
   |\
   | * 72514e3 (feature/board) board complete
   * | 0861a4f master README.md
   |/
   *   92f15f2 Merge branch 'feature/signout'
   |\
   | * 87bcbd2 Complete add
   * | 5a6c6df hotfix
   |/
   * 72c67c4 Complete
   * 7e6c08f Complete menu
   * b056111 Init
   ```

10. branch 삭제

    ```bash
    student@M15016 MINGW64 ~/Desktop/branch (master)
    $ git branch -d feature/board
    Deleted branch feature/board (was 72514e3).
    ```