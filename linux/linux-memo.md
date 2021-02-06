### linux memo

<br/>

- nodejs 설치

```
# 레포지토리 정보 추가
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# nodejs 설치
sudo apt-get install -y nodejs

# 버전확인
node --version
npm --version
```
<br/>
- Java 설치

```
# apt-get 업데이트
sudo apt-get update

# Java 설치
sudo apt-get install openjdk-8-jdk // jdr는 실행만, jdk는 여러 개발도구 포함

# Java 버전 확인
java -version
```

<br/>

- python 최신버전 설치

```
https://www.python.org/ftp/python/3.9.1/
# 리눅스에서 웹브라우저에 접속하여 다운로드 하는 방법
- wget

cd Downloads
sudo wget https://www.python.org/ftp/python/3.9.1/Python-3.9.1.tgz
# 압축된 파일 압축해제
sudo tar xfz Python-3.9.1.tgz
# 빌드 환경 구축 커멘드
sudo ./configure --enable-optimizations
sudo make altinstall



```

<br/>

