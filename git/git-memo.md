#### git에 관련된 메모

> git은 형상관리 Tool

<br/>

- git 전에는 SVN, Perforce 등을 사용

  - 보안상의 이유로 git을 사용하지 않는 회사도 있다

  

- `git add`

  - log 추적을 시작

  - 형상관리 / 로그관리 / 히스토리 관리를 하겠다는 마킹 작업

  - index를 추가하는 작업

    

- `git commit`
  - 추적을 하려는 파일을 로컬 레포지토리에 저장



- `git push`

  - Local에 있던 데이터가 remote로 저장

    

- `git pull`
  - fetch + merge
  - remote에서 Local로 저장



- .gitignore
  - `touch .gitignore`
  - .gitignore을 통해 git으로 관리하지 않을 파일들을 설정한다. (db 등)
  - git init 후에 gitignore.io 사이트에서 프로젝트 관련 보안 사항을 가져와서 .gitignore에 저장





---

<br/>

##### 웬만하면 사용 금지!!

- working dir -> staging area (index) 로 git add 하기 전 제외 (*unstaging*)

  - `git restore HEAD <파일명>`

  - `git reset HEAD <파일명>`    # 과거 버전

    

- staging area (index) 에서 커밋하고 커밋 메시지 수정하는 방법

  - 마지막 커밋만 가능
  - `git commit --amend`



- 마지막 커밋을 취소하는 방법 

  - `git reset HEAD^`
    - 최대한 사용하지 않는것이 좋다. 커밋은 신중하게!
    - 협업을 할 경우 conflict 위험이 높다.
  - `git reset HEAD^^`
    - 커밋 2개 취소
    - `^` 개수에 따라 커밋 개수 삭제

  

- 커밋 기록 삭제 방법

  - reset - commit 기록 자체를 삭제, 내부 내용까지 삭제되고 이전으로 되돌림
  - revert - 삭제할 내용이 지워지지만 기록은 남게됨