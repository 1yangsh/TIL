## vagrant

> 가상화 프로그램인 hyper-v, VirtualBox 등의 VM을 생성하고 지우는 작업을 지원해주는 프로그램

<br/>

- 상태 정보 확인
  - `vagrant status`
- 리눅스 서버 설치 
  - Vagrantfile 작성
  - `vagrant up`
- 서버 중지
  - `vagrant halt <서버 이름>`
  - `vagrant halt` # 서버 전부 중지
- 서버 기동
  - `vagrant up <서버 이름>`
  - `vagrant up` # 서버 전부 기동
- 서버 삭제
  - `vagrant destroy <서버 이름>`
- 서버 접속
  - `vagrant ssh <서버 이름>`
- 서버 재실행
  - `vagrant reload`
- 호스트 네임 변경
  - `sudo vi /etc/hostname`

- ssh 정보 확인
  - `vagrant ssh-config <서버 이름>`

---

- Xshell 연결
  - 새 세션 등록
    - 이름 : Node1
    - 호스트 : 172.0.0.1
    - 포트 번호 : ssh-config에서 확인
  - 사용자 인증
    - 사용자 이름 : vagrant
    - 방법 : Public key 
      - 가져오기 : ssh-config에서 확인

