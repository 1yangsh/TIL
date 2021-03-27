## Ansible

> 오픈 소스 소프트웨어 프로비저닝, 구성 관리, 애플리케이션 전개 도구



## ansible 시작하기

- vagrant를 통한 서버 생성

  - `vagrant init`

    

- Vagrantfile 작성

  ```
  Vagrant.configure("2") do |config|
    config.vm.define:"ansible-server" do |cfg|
      cfg.vm.box = "centos/7"
      cfg.vm.provider:virtualbox do |vb|
          vb.name="Ansible-Server"
          vb.customize ["modifyvm", :id, "--cpus", 2]
          vb.customize ["modifyvm", :id, "--memory", 2048]
      end
      cfg.vm.host_name="ansible-server"
      cfg.vm.synced_folder ".", "/vagrant", disabled: true
      cfg.vm.network "public_network", ip: "172.20.10.10"
      cfg.vm.network "forwarded_port", guest: 22, host: 19210, auto_correct: false, id: "ssh"
      cfg.vm.network "forwarded_port", guest: 8080, host: 58080
      # cfg.vm.network "forwarded_port", guest: 9000, host: 59000
      cfg.vm.provision "shell", path: "bootstrap.sh"  
      # cfg.vm.provision "file", source: "Ansible_env_ready.yml", destination: "Ansible_env_ready.yml"
      # cfg.vm.provision "shell", inline: "ansible-playbook Ansible_env_ready.yml"
      # cfg.vm.provision "shell", path: "add_ssh_auth.sh", privileged: false
  
      # cfg.vm.provision "file", source: "Ansible_ssh_conf_4_CentOS.yml", destination: "Ansible_ssh_conf_4_CentOS.yml"
      # cfg.vm.provision "shell", inline: "ansible-playbook Ansible_ssh_conf_4_CentOS.yml"
    end
  end
  ```

- vagrant 서버 실행
  - `vagrant up`

- vagrant 서버 확인

  - `vagrant status`

  

### 방법 1. Linux 서버에 접속해서 ansible 설치

- ssh 접속
  - `vagrant ssh <server name>`

- net tools 모듈 설치 
  - `sudo yum install -y net-tools`

- 확장 패키지 설치
  - `sudo yum install -y epel-release`

- ansible 설치
  - `sudo yum install -y ansible`

### 방법 2. bootstrap.sh 스크립트에 명령어 작성 후 서버 기동 (권장)

- `bootstrap.sh`

  ```sh
  #! /user/bin/env bash
  
  yum install -y net-tools
  yum install -y epl-release
  yun install -y ansible 
  ```

- ssh 접속

  - `vagrant ssh <server name>`

- ansible 버전 확인
  - `ansible --version`
- Vagrant로 server 생성 (예시)
  - Ubuntu 설치하기

```sh
    config.vm.define:"ansible-node03" do |cfg|
    cfg.vm.box = "ubuntu/trusty64"
    cfg.vm.provider:virtualbox do |vb|
        vb.name="Ansible-Node03"
        vb.customize ["modifyvm", :id, "--cpus", 1]
        vb.customize ["modifyvm", :id, "--memory", 1024]
    end
    cfg.vm.host_name="ansible-node03"
    cfg.vm.synced_folder ".", "/vagrant", disabled: true
    cfg.vm.network "public_network", ip: "172.20.10.13"
    cfg.vm.network "forwarded_port", guest: 22, host: 19213, auto_correct: false, id: "ssh"
    cfg.vm.network "forwarded_port", guest: 80, host: 30080
    # cfg.vm.provision "shell", path: "bash_ssh_conf_4_CentOS.sh"
  end
```
  - Window 설치하기
```sh
# Ansible-Node05 (Windows2012R2)
  config.vm.define:"ansible-node05" do |cfg|
    cfg.vm.box = "opentable/win-2012r2-standard-amd64-nocm"
    cfg.vm.provider:virtualbox do |vb|
        vb.name="Ansible-Node05"
        vb.customize ["modifyvm", :id, "--cpus", 2]
        vb.customize ["modifyvm", :id, "--memory", 2048]
    end
    cfg.vm.host_name="ansible-node05"
    cfg.vm.synced_folder ".", "/vagrant", disabled: true
    cfg.vm.network "public_network", ip: "172.20.10.15"
    cfg.vm.network "forwarded_port", guest: 22, host: 19215, auto_correct: false, id: "ssh"
    cfg.vm.provision "shell", inline: "netsh firewall set opmode disable"
  end

```



---



## Server와 node 2대 생성

- Vagrantfile 생성

  ```sh
  Vagrant.configure("2") do |config|
    config.vm.define:"ansible-node01" do |cfg|
      cfg.vm.box = "centos/7"
      cfg.vm.provider:virtualbox do |vb|
          vb.name="Ansible-Node01"
          vb.customize ["modifyvm", :id, "--cpus", 1]
          vb.customize ["modifyvm", :id, "--memory", 1024]
      end
      cfg.vm.host_name="ansible-node01"
      cfg.vm.synced_folder ".", "/vagrant", disabled: false
      cfg.vm.network "public_network", ip: "172.20.10.11"
      cfg.vm.network "forwarded_port", guest: 22, host: 19211, auto_correct: false, id: "ssh"
      cfg.vm.network "forwarded_port", guest: 80, host: 10080
      cfg.vm.provision "shell", path: "bash_ssh_conf_4_CentOS.sh"
    end
  
    config.vm.define:"ansible-node02" do |cfg|
      cfg.vm.box = "centos/7"
      cfg.vm.provider:virtualbox do |vb|
          vb.name="Ansible-Node02"
          vb.customize ["modifyvm", :id, "--cpus", 1]
          vb.customize ["modifyvm", :id, "--memory", 1024]
      end
      cfg.vm.host_name="ansible-node02"
      cfg.vm.synced_folder ".", "/vagrant", disabled: false
      cfg.vm.network "public_network", ip: "172.20.10.12"
      cfg.vm.network "forwarded_port", guest: 22, host: 19212, auto_correct: false, id: "ssh"
      cfg.vm.network "forwarded_port", guest: 80, host: 20080
      cfg.vm.provision "shell", path: "bash_ssh_conf_4_CentOS.sh"
    end
  
    config.vm.define:"ansible-server" do |cfg|
      cfg.vm.box = "centos/7"
      cfg.vm.provider:virtualbox do |vb|
          vb.name="Ansible-Server"
          vb.customize ["modifyvm", :id, "--cpus", 2]
          vb.customize ["modifyvm", :id, "--memory", 2048]
      end
      cfg.vm.host_name="ansible-server"
      cfg.vm.synced_folder ".", "/vagrant", disabled: false
      cfg.vm.network "public_network", ip: "172.20.10.10"
      cfg.vm.network "forwarded_port", guest: 22, host: 19210, auto_correct: false, id: "ssh"
      cfg.vm.network "forwarded_port", guest: 8080, host: 58080
      # cfg.vm.network "forwarded_port", guest: 9000, host: 59000
      cfg.vm.provision "shell", path: "bootstrap.sh"  
      # cfg.vm.provision "file", source: "Ansible_env_ready.yml", destination: "Ansible_env_ready.yml"
      # cfg.vm.provision "shell", inline: "ansible-playbook Ansible_env_ready.yml"
      # cfg.vm.provision "shell", path: "add_ssh_auth.sh", privileged: false
  
      # cfg.vm.provision "file", source: "Ansible_ssh_conf_4_CentOS.yml", destination: "Ansible_ssh_conf_4_CentOS.yml"
      # cfg.vm.provision "shell", inline: "ansible-playbook Ansible_ssh_conf_4_CentOS.yml"
    end
  end
  ```

- `bash_ssh_conf_4_CentOS.sh`

  ```sh
    #! /usr/bin/env bash
  
  now=$(date +"%m_%d_%Y")
  cp /etc/ssh/sshd_config /etc/ssh/sshd_config_$now.backup
  sed -i -e 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
  systemctl restart sshd
  ```

- 서버 실행

  - `vagrant up`

- 서버와 노드에서 ping 테스트

  - `ping <서버ip or 노드ip>`



- host 파일에 그룹 추가 (server node)

  - `sudo vi /etc/ansible/hosts`

    ```
      :
      :
    [nginx]
    172.20.10.11  // node01의 ip
    172.20.10.12  // node02의 ip
    ```

    

- hostname과 ip의 매핑 정보 등록

  - `sudo vi /etc/hosts`

    ```
      :
      :
    172.20.10.10 ansible-server
    172.20.10.11 ansible-node01
    172.20.10.12 ansible-node02
    ```

  - `ping ansible-node01`  

    - `ping <host name>` 명령이 가능해짐



- Server에서 node 접속
  - `ssh <node name>`



- key pair 생성 (비대칭 키)

  - `ssh-keygen`

  - 키 파일 확인

    - `ls -al ~/.ssh`

  - 각 노드로 key copy

    - `ssh-copy-id root@ansible-node01`
    - `ssh-copy-id root@ansible-node02`
    - `ssh-copy-id vagrant@ansible-node01`
    - `ssh-copy-id vagrant@ansible-node01`

  - 각 노드로 ping 정보 전달

    - `ansible all -m ping`

      

> 키 값을 복사해 놓음으로써 server에서 node로 ssh 접속 시, 패스워드 인증 절차를 생략하게 됨



- 서버 생성시간 확인하기 
  - `ansible all -m shell -a "uptime"`
- 각 노드가 할당하고 있는 메모리 확인
  - `ansible all -m shell -a "free -h"`



### 사용자 생성

- 사용자 확인
  - `cat /etc/passwd`
- 모든 노드에 사용자 추가
  - `su -`
  - `ansible all -m user -a "user=<username> password=<pwd>" -k`



### server에서 해당 그룹으로 파일 또는 디렉터리 복사

- test 파일 작성
- `ansible <해당 그룹> -m copy -a "src=./<복사할 파일> dest=/home/varant"`



### ansible에서 모듈 사용법

> https://docs.ansible.com/ansible/2.8/modules/list_of_all_modules.html

- `ansible all -m <module> ...`