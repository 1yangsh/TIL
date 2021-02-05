## Cloud memo



```
웹서버 설치
sudo apt-get install -y apache2

웹 서비스 연결
sudo service apache2 start

웹서비스 상태 확인
sudo service apache2 status

net-tools 설치
sudo apt-get install net-tools

현재 사용중인 port정보
netstat -ntpl
```

<br/>



AWS EC2 이용하기

- VPC 발급
- EC2 서버 만들기

```
# 키페어 권한 변경
chmod 400 <key>
```



```
powershell에서 접속하는 방법
ssh -i <키값> <계정명>@<로컬ipv4>
```

