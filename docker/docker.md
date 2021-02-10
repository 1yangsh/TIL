## docker

> 컨테이너 가상화를 구현하기 위한 기술

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
    - `docker container run --rm -it ubuntu:16.04 /bin/bash `

<br>

- MySQL 실행

  - Mysql 컨테이너 run
    - `docker container run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7 `
      - -d = detached mode
      - -p = port forwarding
        - 3306(host) : 3306(container)
      - -e = environment
  - mysql 컨테이너 실행
    - `docker container exec -it mysql /bin/bash`
  - 컨테이너 내부의 mysql 실행
    - `mysql -h127.0.0.1 -uroot -p`
  - bash 쉘 실행 + mysql 실행
    - `docker container exec -it mysql /bin/bash -c mysql -h172.0.0.1 -uroot -p`

<br/>

- 일괄적인 삭제 방법 (image, container, volume 등)
  - `docker system prune`
  - 컨테이너 삭제
    - `docker container prune`
  - 이미지 삭제
    - `docker image prune`

<br/>

---

<br/>

### volume mount 지정

- Volume mount 지정
  
- `-v [Windows의 경로]:[Container의 경로]`
    - 예) `-v C:\Work\my_test:/mydata`
  - `docker run --volume D:\docker_volume:/var/lib/mysql -d -p 13306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7 `
  
- Volume 확인

  - `docker volume ls`

  

<br/>

### docker image 파일 생성

- docker 파일 생성

  ```
  FROM ubuntu:latest // base 파일 생성
  ```

- image 파일 생성

  - `docker image build --tag fromtest:1.0 .`
  - = `docker build -t <name>:<tag> .`

<br/>

#### CMD와 Entrypoint 차이

> 결과 값은 같다

- CMD
  - 파라미터에 의해 변경될 수 있는 가변 데이터
- Entrypoint
  - 고정적인 값을 넣고 싶을 때

<br/>

- 포트포워딩
  - inbound를 오픈한다
    - `expose 8080`
  - 다른 컨테이너에서 접속 방법(리눅스)
    - `curl -X GET http;//<주소>`
  - 윈도우(호스트OS)에서 접속 방법 (포트포워딩)
    - `docker run -p 8080:8080 -d mynodejs`

<br/>

---

<br/>

### docker login

- login
  - `docker login`
- image 업로드
  - `docker image push 1yangsh/mynodejs:1.0`

<br/>

### docker network

- `docker network ls`
  - 네트워크 확인
- `docker network create <네트워크 이름>`
  - 네트워크 생성

- `docker network connect <네트워크 이름>  <컨테이너 id>`
  - 컨테이너를 해당 네트워크에 연결
- `docker network inspect <네트워크>`
  - 네트워크 정보 확인

<br/>

```
예) 
1. 컨테이너 2개 실행 / 같은 네트워크 연결
	- mysql
	- mynode
2. mynode에서 mysql을 ping 테스트
	- docker -it [container_id] /bin/sh
    - ping 172.17.0.2
    - ping mysql
   
```









- mysql 실행
  - `docker run -d -p 13306:3306 --network my-network --name mysql_server 1yangsh/mymysql:latest`
- django 실행
  - `docker run -d -p 8000:8000 --network my-network --name mybookmark_app 1yangsh/mydjango:latest`