## docker

<br/>

- 시작하기

  -  image 다운, 컨테이너 실행
    - `docker container run -d -p 80:80 docker/getting-started`
  
- image 확인하기
  - `docker image ls`
  - container 확인
    - `docker container ls`
    - `docker ps`
      - 작동 중인 컨테이너만 보여줌
  - `docker container ls -a`
      - 모든 컨테이너 보여줌

  <br/>

  

  ### Image

  - image 다운로드
    - `docker image pull <image 네임>`

  

  ### Container

  - container 실행하기
    - `docker container start <컨테이너 id>`

  -  container 중지하기
     
  -  `docker container stop <컨테이너 id>`
     
  - container 삭제
    
    - `docker container rm <컨테이너 id>`
    
  -  이미지 다운 & 컨테이너 실행

     -  `docker container run <image>`

     

<br>

- Ubuntu 컨테이너 실행
  - 터미널을 생성해서 우분투 컨테이너 실행
    - `docker container run -it ubuntu:16.04 /bin/bash `
  - 컨테이너 Stop시, 컨테이너 삭제 옵션까지 실행
    - ``docker container run --rm -it ubuntu:16.04 /bin/bash `

<br>

- MySQL 실행

  - Mysql 컨테이너 run
    - `docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7 `
      - -d = detached mode
      - -p = port forwarding
        - 3306(host) : 3306(container)
      - -e = environment
  - mysql 컨테이너 실행
    - `docker container exec -it mysql /bin/bash`

  - 컨테이너 내부의 mysql 실행
    - `mysql -h127.0.0.1 -uroot -p`

