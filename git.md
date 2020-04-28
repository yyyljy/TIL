# git

- 분산 버전 관리 시스템
- 공간
  - Working Directory
    - add : Staging area로
  - Staging area
    - commit
    - rm --cached : Working Directory로
    - restore --staged : Working Directory로
  - commit

## 기초명령어

### CLI 기초 명령어

```bash
student@M15016 MINGW64 ~/Desktop/test (master)
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

student@M15016 MINGW64 ~/Desktop/test (master)
$ touch a.txt

student@M15016 MINGW64 ~/Desktop/test (master)
$ ls
a.txt

student@M15016 MINGW64 ~/Desktop/test (master)
$ cd

student@M15016 MINGW64 ~
$
```

### 상황

1. add

   ```bash
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git status
   On branch master
   
   No commits yet
   # 트래킹 X. 새로 생성된 파일.
   Untracked files:
   	# 커밋을 하기 위한 곳에 포함시키려면
   	# Staging area로 이동시키려면, git add
     (use "git add <file>..." to include in what will be committed)
           a.txt
   # WD O, Staging area X
   nothing added to commit but untracked files present (use "git add" to track)
   
   ```

2. git status

   ```bash
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git rm --cached a.txt
   rm 'a.txt'
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git status
   On branch master
   
   No commits yet
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           a.txt
   
   nothing added to commit but untracked files present (use "git add" to track)
   
   ```

   ```bash
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git add a.txt
   $ git status
   On branch master
   # 커밋될 변경사항들(staging area O)
   No commits yet
   
   Changes to be committed:
     (use "git rm --cached <file>..." to unstage)
           new file:   a.txt
   
   ```

   ```bash
   $ git commit -m 'Create a.txt'
   [master (root-commit) f7af3e8] Create a.txt
    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 a.txt
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git log
   commit f7af3e86b736f07a02606ed87963627283ffe8a0 (HEAD -> master)
   Author: yyyljy <yyyljy@gmail.com>
   Date:   Thu Apr 23 10:37:17 2020 +0900
   
       Create a.txt
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git log --oneline
   f7af3e8 (HEAD -> master) Create a.txt
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git status
   On branch master
   nothing to commit, working tree clean
   ```

3. 추가 파일 변경 상태

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   a.txt
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   ```

   ```bash
   $ git add a.txt
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git status
   On branch master
   Changes to be committed:
     (use "git restore --staged <file>..." to unstage)
           modified:   a.txt
   
   Untracked files:
     (use "git add <file>..." to include in what will be committed)
           b.txt
   ```

4. 커밋 취소

   ```bash
   $ git commit --amend
   ```

   - vim 텍스트 편집기가 실행된다.

   - 특정 파일을 빼놓고 커밋 했을 때

     ```bash
     $ git add <omit_file>
     $ git commit --amend
     ```

     - 빠트린 파일을 add한 이후에 commit --amend 를 하면, 해당 파일까지 포함하여 재커밋이 이뤄진다.

5. 작업 내용을 이전 버전으로 되돌리기

   - 작업하던 내용 버리기

   ```bash
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   a.txt
           modified:   b.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git restore a.txt
   
   student@M15016 MINGW64 ~/Desktop/test (master)
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   b.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   ```

6. 특정 파일/폴더 삭제 커밋

> 해당 명령어는 실제 파일이 삭제되는 것은 아니지만, git에서 삭제되었다라는 이력을 남기는 것.

​	일반적으로 gitignore와 함께 사용한다.

## gitignore

- git으로 관리하고 싶지 않은 파일을 등록하여 활용할 수 있다.
- 일반적으로 프로젝트 환경(IDE, OS 등)에 관련된 정보나 추가적으로 공개되면 안되는 데이터파일 등을 설정한다.
- 이란 프로젝트 환경에 대한 정보는 우선 [gitignore.io](https://gitignore.io)에서 프로젝트 시작할 때 마다 정의하는 습관을 가지자.

```bash
# 특정 파일
secret.csv
# 특정 폴더
idea/
# 특정 확장자
*.csv
# 특정 폴더에서 특정 파일 빼고
!idea/a.txt
```

```bash
$ git remote add origin url
$ git remote -v
$ git remote rm

```

- [git commit msg를 위한 영어 사전](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)

- [markdown을 위한 이모지](https://gist.github.com/rxaviers/7360908)

- | 여기                                            | https://bit.ly/bigiota                                       |
  | ----------------------------------------------- | ------------------------------------------------------------ |
  | 수업 정리                                       | https://github.com/edutak/bigiot-a                           |
  |                                                 |                                                              |
  | 개발자 기술 면접                                | https://github.com/JaeYeopHan/Interview_Question_for_Beginner |
  | 주니어 개발자 채용 정보                         | https://github.com/jojoldu/junior-recruit-scheduler          |
  | 원격 근무 회사                                  | https://github.com/milooy/remote-or-flexible-work-company-in-korea |
  | 좋은 git commit 메시지를 위한 영어 사전         | https://blog.ull.im/engineering/2019/03/10/logs-on-git.html  |
  | 백엔드 개발자를 꿈꾸는 학생개발자에게           | https://d2.naver.com/news/3435170                            |
  | 좋은 git 커밋 메시지를 작성하기 위한 7가지 약속 | https://meetup.toast.com/posts/106                           |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |
  |                                                 |                                                              |