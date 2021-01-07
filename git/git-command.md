# git command

> git을 사용하기 위한 기본 명령어



## 설정, 상태

### 1. init

- `git init`

- 현재 폴더를 git으로 관리하겠다. => `.git` 폴더를 생성
- 최초에 한번만 실행하는 명령어



### 2. config

- `git config --global user.email "email@email.com"`

- `--global` 옵션과 `--local` 둘중 하나 선택하여 사용

  - 일반적으로 global 설정을 해 놓으면 내 컴퓨터에서 추가적으로 변경할 일이 없음

  - 하지만 공용으로 사용하는 pc, 회사의 pc라면 local 설정 필요

    

### 3. status

- `git status`
- 현재 상태를 출력



### 4. diff

- `git diff`
- **마지막 커밋**과 **현재 폴더** 상태를 비교해서 차이점을 출력



### 5. log

- `git log`
- 커밋 히스토리를 출력



### 6. remote add

- `git remote add origin <url>`
- 깃아, 원격저장소 추가해줘 별명은 origin이고 실제주소는 <url>이야 





## 저장

### 1. add

- `git add <추가하려고 하는 파일>`
  - `git add .  ` : 한번에 모든 파일과 폴더를 add
- working directory에서 변경점을 `staging area(index)`로 이동



### 2. commit

- `git commit -m "메시지"` : 커밋과 커밋에 관한 메세지까지 한번에 남김

  

### 3. push

- `git push origin master`

