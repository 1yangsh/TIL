## 소스코드 배포

> p25~42

### #1 git을 이용한 소스코드 배포

- git 설치에 필요한 패키지를 설치
  
- $ `sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel`
  
- 작업 디렉터리 생성 후 이동
  - $ `cd /var`
  - $ `sudo mkdir www`
  - $ `sudo chown ec2-user www`
  - $ `cd /var/www`

- github으로부터 소스코드를 가져옴
  - $ `git clone https://github.com/deopard/aws-exercise-a.git`
  - $ `cd aws-exercise-a/`

- 소스코드를 확인

  - $ `ll`

    ```
    -rw-rw-r-- 1 ec2-user ec2-user   294 Mar  8 05:56 app.js
    -rw-rw-r-- 1 ec2-user ec2-user 35149 Mar  8 05:56 LICENSE
    -rw-rw-r-- 1 ec2-user ec2-user   539 Mar  8 05:56 package.json
    -rw-rw-r-- 1 ec2-user ec2-user 13432 Mar  8 05:56 package-lock.json
    drwxrwxr-x 2 ec2-user ec2-user    19 Mar  8 05:56 public
    ```

  - $ `cat app.js`

    ```javascript
    const express = require('express');							//	⇐ 웹 서버 모듈을 포함
    const app = express();
    
    app.get('/', (req, res) => {								//	⇐ / 로 GET 방식의 요청이 들어오면 
      res.send('AWS exercise의 A project입니다.'); 				  //     지정된 메시지를 응답 본문으로 반환
    });
    
    app.listen(3000, () => {									// ⇐ 3000번 포트로 요청을 대기
      console.log('Example app listening on port 3000!');	
    });
    
    app.get('/health', (req, res) => {							// /health 로 GET 방식의 요청이 들어오면
      res.status(200).send(); 						 		   // 응답 헤더의 상태코드의 값을 200으로 설정해서 반환
    });
    ```

  - $ `cat package.json`

    ```javascript
    {
        "express": "^4.16.3"					//	⇐ express 의존 모듈로 등록되어 있음
      }
    ```

- 의존 모듈 설치

  - $ `npm install`

    

### #2 웹 서버와 웹 애플리케이션 서버로 이원화

- Web server / Web Application Server
  - 웹서버
    - 정적 자원 요청에 대한 응답
    - *nginx*
  - 웹 애플리케이션 서버
    - 응용 프로그램의 실행 결과를 반환
    - *Phusion Passenger*

- Phusion Passenger 설치 파일 다운로드
  
- $ `wget https://s3.amazonaws.com/phusion-passenger/releases/passenger-5.3.6.tar.gz`
  
- 작업 디렉터리 생성 및 압축 해제
  - $ `sudo mkdir /var/passenger`
  - $ `sudo chown ec2-user /var/passenger`
  - $ `tar -xzvf passenger-5.3.6.tar.gz -C /var/passenger/`

- 경로 설정

  - $ `echo export PATH=/var/passenger/passenger-5.3.6/bin:$PATH >> ~/.bash_profile`
  - $ `source ~/.bash_profile`

- rvm 설치

  - $ `gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`

  - $ `curl -sSL https://get.rvm.io | bash -s stable`
  - $ `source ~/.rvm/scripts/rvm`
  - $ `rvm reload`
  - $ `rvm install 2.4.3`

- passenger nginx-module 설치
  - $ `passenger-install-nginx-module`
    - node.js만 선택한 후 install과정은 메모리가 필요하므로 ctrl+c로 중단

- 가상 메모리를 증설
  - $ `sudo dd if=/dev/zero of=/swap bs=1M count=1024`
  - $ `sudo mkswap /swap`
  - $ `sudo swapon /swap`

- 재설치
  - $ `passenger-install-nginx-module`
    - 권한 오류 발생
- rvmsudo를 이용해서 루트 권한으로 설치를 진행
  - $ `export ORIG_PATH="$PATH"`
  - $ `rvmsudo -E /bin/bash`
  -  `# export PATH="$ORIG_PATH"`
  - `# export rvmsudo_secure_path=1`
  - ` # /home/ec2-user/.rbm/gems/ruby-2.4.3/wrappers/ruby /var/passenger/passenger-5.3.6/bin/passenger-install-nginx-module`
    - 다시 설치 진행 (20~30분 소요)

- root에서 ec2-user 계정으로 전환

  - `exit`

- nginx.conf 수정

  - $ `sudo vi /opt/nginx/conf/nginx.conf`

    ```nginx
    worker_processes  1;
    
    events {
        worker_connections  1024;
    }
    
    
    http {
    	server_names_hash_bucket_size 256;
        passenger_root /var/passenger/passenger-5.3.6;
        passenger_ruby /home/ec2-user/.rvm/gems/ruby-2.4.3/wrappers/ruby;
    
        include       mime.types;
        default_type  application/octet-stream;
    
        sendfile        on;
        #tcp_nopush     on;
    
        #keepalive_timeout  0;
        keepalive_timeout  65;
    
        #gzip  on;
    
        server {
            listen                  80;
            server_name             34.224.83.104;
            root                    /var/www/aws-exercise-a/public;
            passenger_enabled       on;
            passenger_app_type      nodes;
            passenger_startup_file  /var/www/aws-exercise-a/app.js;
        }
    }
    ```

- nginx 서비스를 시작

  - $ `sudo /opt/nignx/sbin/nginx`

  ![img](2-aws-소스코드배포.assets/eGw0FDevxBXIdet2eTkfhNMbFhWYLzWiyGJb6MVwzWJXF6u_CvcprQBwYqwqVyl2tqIJlQDoN4JHzBStGmFZVPY0ktJjtRfRJSd5Lb9nLFs3jzEfVET0qGq0AY5bmrx0q9G3VZoA)

  - nginx 구동
    - `sudo /opt/nginx/sbin/nginx`
  - nginx 중지
    - `sudo /opt/nginx/sbin/nginx -s stop`
  - nginx 재구동
    - `sudo /opt/nginx/sbin/nginx -s reload`



### #3 nginx, Phusion Passenger 서비스 명령어 추가

- /etc/init.d 경로(서비스 스크립트가 존재하는 경로)에 스크립트를 추가

  - $ `sudo vi nginx`

  ```nginx
  #!/bin/sh
  #
  # nginx - this script starts and stops the nginx daemin
  #
  # chkconfig:   - 85 15 
  # description:  Nginx is an HTTP(S) server, HTTP(S) reverse \
  #               proxy and IMAP/POP3 proxy server
  # processname: nginx
  # config:      /opt/nginx/conf/nginx.conf
  # pidfile:     /opt/nginx/logs/nginx.pid
  # modified from http://articles.slicehost.com/2009/2/2/centos-adding-an-nginx-init-script
  
  # Source function library.
  . /etc/rc.d/init.d/functions
  
  # Source networking configuration.
  . /etc/sysconfig/network
  
  # Check that networking is up.
  [ "$NETWORKING" = "no" ] && exit 0
  
  nginx="/opt/nginx/sbin/nginx"
  prog=$(basename $nginx)
  
  NGINX_CONF_FILE="/opt/nginx/conf/nginx.conf"
  
  lockfile=/var/lock/subsys/nginx
  
  start() {
      [ -x $nginx ] || exit 5
      [ -f $NGINX_CONF_FILE ] || exit 6
      echo -n $"Starting $prog: "
      daemon $nginx -c $NGINX_CONF_FILE
      retval=$?
      echo
      [ $retval -eq 0 ] && touch $lockfile
      return $retval
  }
  
  stop() {
      echo -n $"Stopping $prog: "
      killproc $prog -QUIT
      retval=$?
      echo
      [ $retval -eq 0 ] && rm -f $lockfile
      return $retval
  }
  
  restart() {
      configtest || return $?
      stop
      start
  }
  
  reload() {
      configtest || return $?
      echo -n $"Reloading $prog: "
      killproc $nginx -HUP
      RETVAL=$?
      echo
  }
  
  force_reload() {
      restart
  }
  
  configtest() {
    $nginx -t -c $NGINX_CONF_FILE
  }
  
  rh_status() {
      status $prog
  }
  
  rh_status_q() {
      rh_status >/dev/null 2>&1
  }
  
  case "$1" in
      start)
          rh_status_q && exit 0
          $1
          ;;
      stop)
          rh_status_q || exit 0
          $1
          ;;
      restart|configtest)
          $1
          ;;
      reload)
          rh_status_q || exit 7
          $1
          ;;
      force-reload)
          force_reload
          ;;
      status)
          rh_status
          ;;
      condrestart|try-restart)
          rh_status_q || exit 0
              ;;
      *)
          echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
          exit 2
  esac
  ```

  - 권한 변경
    - $ `sudo chmod 755 nginx`
  - nginx 서비스 중지
    - $ `sudo service nginx stop`
  - nginx 서비스 실행
    - $ `sudo service nginx start`
  - 서비스 상태 확인
    - $ `sudo service nginx status`

- 시스템 시작 시 자동 시작 서비스 등록

  - $ `sudo chkconfig --add nginx`

  - $ `sudo ntsysv`

    ![img](2-aws-소스코드배포.assets/jZ_OqoLcyk8DQlqGpKet5mc3NrA-W0Rnlt4c1YdIH00xC4OeAPfcgxvb52ZxpzE3_Ch-cW3Civc8X-E7A_jTOD5kr1d-Bn5HexROV9JMG4qmYjXg2heiIxmShJcI02p6M6xaJWhQ)



### #4 하나의 서버에서 두 개의 애플리케이션을 서비스

- 애플리케이션을 추가 설정

  - $ `cd /var/www`

  - $ `git clone https://github.com/deopard/aws-exercise-b.git`

  - $ `cd aws-exercies-b`

  - $ `ll`

    ```
    total 24
    -rw-rw-r-- 1 ec2-user ec2-user   294 Mar  8 07:47 app.js
    -rw-rw-r-- 1 ec2-user ec2-user   539 Mar  8 07:47 package.json
    -rw-rw-r-- 1 ec2-user ec2-user 13432 Mar  8 07:47 package-lock.json
    drwxrwxr-x 2 ec2-user ec2-user    19 Mar  8 07:47 public
    ```

  - $ `cat app.js`

    ```javascript
    const express = require('express');
    const app = express();
    
    app.get('/', (req, res) => {
      res.send('AWS exercise의 B project입니다.');
    });
    
    app.listen(3000, () => {
      console.log('Example app listening on port 3000!');
    });
    
    app.get('/health', (req, res) => {
      res.status(200).send();
    });
    ```

  - $ `npm install`

    - aws-exercise-a 모듈을 실행할 때 이미 필요한 것을 모두 설치했기 때문에 설치할 모듈이 적다

- aws-exercise-b로 라우팅될 수 있도록 nginx.conf 파일을 server 요소를 추가

  - $ `sudo vi /opt/nginx/conf/nginx.conf`

    ```nginx
    // 추가
    server {
            listen                  80;
            server_name             ec2-34-224-83-104.compute-1.amazonaws.com;
            root                    /var/www/aws-exercise-b/public;
            passenger_enabled       on;
            passenger_app-type      node;
            passenger_startup_file  /var/www/aws-exercise-b/app.js;
        }
    ```

- 서버 재실행 후 접속 테스트

  - $` sudo service nginx restart`

  ![img](2-aws-소스코드배포.assets/dqXHNYgbVQ8T8L4Yg9Klm1aYvMfWYDp3RHYvPWYFTG2PrTZ7t9nuLpI_qutVD9jy83s7qdFPhKmFsDps4rwGISh2py4GgS0i6nQ3o92aI16Rt8DlUQoO_EhBOYmZI66UTFbf0B4b)

  ![img](2-aws-소스코드배포.assets/8wZQnJ-dp4kKGNWEjdLB1WeTHKmVVLbXYcKiTVp98TryXAMEiJVFdScXvx71FVHECb_lXzJPZ-kebLYFEkGrG8ZjAstQtsad3mQwoHkn0nH_rvd10R6NRdXV-_c5F9FelTDOOa2T)