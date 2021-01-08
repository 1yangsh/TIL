## git branch

> git branch 관련 명령어



#### git branch

- branch 생성

  - `git branch <브랜치명>`

    

- branch 변경

  - `git switch <브랜치명>`       # git 2.23 이후 - 현재 권장

  - `git checkout <브랜치명`    # 2.23이전 명령어

    

- 현재 branch 상태

  - `git branch`

  - `git branch -v`             # 최신 커밋 기록 같이 출력

    

- branch 삭제

  - `git branch -d <브랜치명>`     # 현재 자신의 브랜치는 삭제 불가



- branch 병합
  - `git merge <다른 브랜치명>`
  - 메인 브랜치로 이동해서 다른 브랜치에서 커밋한 것을 병합한다.



- log 상태 확인
  - `git log`
  - `git log --oneline` # 한줄로 로그 기록 출력
  - `git log --oneline --graph`  # 로그 기록을 graph 형태로 출력





