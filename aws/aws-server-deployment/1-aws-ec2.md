## EC2 인스턴스 생성 및 서버 환경 구성 

> AWS 인프라 구축 가이드 (p13~25)

### #1 리전 확인 및 설정

​	![img](1-aws-ec2.assets/cdEGBjY-ZsaXoD6xL8mvsrvFr91wlXZsKvOS-MfkFu3e4PiRW21YtXrZv0jQagYNGlNbHAvKqDtRL8fj2TBy1OBFnQrsHqrnYB5F-ipkyNErRTCpoYmSxYGbTk8Lut1mChIDziAk)



### #2 EC2 인스턴스 생성

- [인스턴스 시작] 버튼 클릭

  ![img](1-aws-ec2.assets/OJNQ5Odat3UASbSb6bO36Qs-CAZqaO71nEGHj_E9ap13hda4iE4iq_XhYH9AN03u3AK571qpoMbmHzxwXTz_-rPq_S8rVgFwWArD04xquT3HB7gbt4CqiDsjdwIRD-ri0Im4z5pP)

  ---

  ![img](1-aws-ec2.assets/hYRXkVXGn0x3NwgpiWx3EHKk0ya86RrqnpHWtC-2IRUXacte1A9CpmTjqnJZBFbZrUXHPUuxD8Ag72GgYlHZ9kkoAFZVgU2tyg257-n8qSwouwaLcjXO0XYBPBVJcyfyl8PmBc_S)

  ---

  ![img](1-aws-ec2.assets/pgOAE8yjc3Ku7bBCWJihB0lmYvgvGOOUqi7Vuz7ksMvY5DeFpiRnMV1zrJnfot_BkXPy8XOTuJ1kDAv9uvk1cPglfIfm11whVlDpR1E43Za4cahTqMQjZEIoYCaPXkayyR2vO1Nu)

  ---

  ![img](1-aws-ec2.assets/bv1c_4MMdYQl-bx5Dm15BiFit1klzQ9prnrpoHO9Qh5Vgd0Y9ielU35E0jPE8pA3DH6ql0uD-xosXujAqtlT9cdIUxlF99Tw3tY-x_Zk5p-AWOl74AuWeSD90VeMgUK2zvf_moyr)

  ---

  ![img](1-aws-ec2.assets/2bSsQfrjr3blPaL48bKMOKn-oXMVnvk2W212lO0AQcimfwmUxoHssO-BoLs3fZ_TbWEIUSexrEtGjbQLh4bAAhJG_Xrl0n9c0AZbziyvGU4ASNLY-ZwWUGIlG9qcuOQVeRxEeJl6)

  ---

  ![img](1-aws-ec2.assets/mSozDgyyYMh2u3ZpnCUVGLvTkZgbRCQ0YuMrtl8cErLxyggGvEZFrnN9TgUFWrYcSDjelE3NLmuQcsc6w1CRgKduwGhOQp7qSOlDmgaSdEspiNOaglp9DZ4aFXxaa0Jiav1roceY)

  ---

  ![img](1-aws-ec2.assets/YgPErWBq6NBkkMjfIAZFHhphYW7zic-mRUOzoCwh0ZJXIdSKskVgA5yfAWyuk2vYjGtQPX9RgHBRzOMjHL8ggBvF6DjzE-Ylss-adeKeIrCvI6iIknSUAlD7XuzDBtoNFmcHdw9C)

  ---

  ![img](1-aws-ec2.assets/Rfwpj489P-lq6BrG2qFjV-T2KD4RPcDWg4RtMHoaFm9Vl4MowbwXnqmMuFTnjZvzbelU-3HbqTEC-ew9J2QNKKoPhUErDqK_zvB_cc4qRCrwg2wLMopvTQ2bIJq85M1rUJBBuY1g)



### #3 SSH 접속을 위한 개인키 등록 및 SSH 접속

- Bitvise SSH client를 이용한 SSH 접속

  ![img](1-aws-ec2.assets/mUVhb5F7j0fu_tbS1afeiWFb2_apsZoLHa6IrH9B-OT8_YMowMzoJGs9jAEb8RHv9nbQQk83GveS8iSk5PVNetwTPGV5Cm6gjJTxnYh4T7kTdNlBuh95Bh5vuN3dyAZ9leGsRz90)

  ---

  ![img](1-aws-ec2.assets/fbbYLG1eBJkNpDmLyuXf8Qw25wfAU7ABHM_eFTAv-nyVpvx9UpyW1YNC91tWV-N4WkUZKaljLdcRKkUeU1K_lm_BNhxGvYHPT8XYR7arfNEVWoCvHCQz9wd0LNOubTcbbheJvNIt)

  ---

  ![img](1-aws-ec2.assets/5t62HpgEOD2LftoRWE0uhuukXktXESItczha50OejFpAS9eozxCOy9WvA2_XgS9S7UQ9gWxdtjKEifop448kS67_QD4fJj4oc35Qm9HGzT9AhnvNZSCs7fGc-GZaCH6LRtkgOMpZ)



### #4 HTTP, HTTPS 접속이 가능하도록 보안그룹을 추가

- VPC

  ![img](1-aws-ec2.assets/YC04jG2r97k-KFkABC7sNEJVOHjb9Bmir12-1eefWwQZW91Zpo49ARJoGLf8Hq8VHL1cPcOok3bMZQpecOnXLr2i2luid_jiYvPsviXs4IBkhim7rfcp21j4RDQF4-U3FrXXGykD)

  ---

  ![img](1-aws-ec2.assets/90Ntq1rcTrx47Sg7EgxJ8GCf58vihXfAqEz89hX7DYmQ_AoizrdurGaqXnOmT1McDADr2-_pXT31uOxUvibzEevX1cUeix4K1VeSUMkxV64UWpJaL8SJ4BIHnOXTx__c_dLqI8r-)

  ---

  ![img](1-aws-ec2.assets/SOWQah-H773TFD_W94lVw5OQ4Six3gCdPMSGmkIy11ycrj3PXWlMzUUySwsjPhCm2zzdwlCuGmuNvcX9jBJgIQvRK8k_IW6RC_d90Yo4pfButrgXVm1dl9agh1W3x0WDuiiaOERW)

  ---

  ![img](1-aws-ec2.assets/OScBenTQGwryaLGjjgZI_5_4GkpS0PHjLPT73Btel16Y5y_prVQQrzZYnpF7pDdvt04pgw5sWfgSUcFIpCPHGnqa5fWAhwLYy1nzrHKKRO_efCu3GOBgQTAuJKp7TxdjmIAtpI2A)

  ---

  ![img](1-aws-ec2.assets/CL4J_3vkz8oJ4qcXDUl-jlIO6LPFBxe3T_RqATUuRMx3DqCCkhQW2zb1TgvzewL8NxmkQhGvUn380GQwEEKz-TkgMV9US-gYow6M9ke2-JgEGCy2aLsO_4vzmPW6vzSDqILVqooV)

  ---

  ![img](1-aws-ec2.assets/cE7BheYc9a0NTtkx6Cu_L2HeoWGNfVpISqIvS2-o0XztbUt92cpzDWnPGt0FNNT3dG0n2Z3anWl7t9pTfcArBKLYlxYddbD5YSnCW8xdAAjzwPYrqBMVAvsnJq9H5V8WBrAGfnTW)

  ---

  ![img](1-aws-ec2.assets/balZRDxJv-BWXgfPCgsk35nfAo4y62ymATcac5zQs-WdVGDP_LPiU7P4P6b1x1JUTxJvYF3BPSINYbF4OE_fnjMjDJ-rUfTHUkMXbRiGy8YzhXCv3ZW9jlYkBms0QPCWtavTaHAQ)



### #5 서버 환경 구성 - 실습에 필요한 node.js 설치

- nvm(Node Version Manager) 설치 스크립트를 가져와서 실행(설치)

  - $ `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash`

    ```
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100 12819  100 12819    0     0   596k      0 --:--:-- --:--:-- --:--:--  596k
    => Downloading nvm as script to '/home/ec2-user/.nvm'
    
    => Appending nvm source string to /home/ec2-user/.bashrc
    => Appending bash_completion source string to /home/ec2-user/.bashrc
    => Close and reopen your terminal to start using nvm or run the following to use it now:
    
    export NVM_DIR="$HOME/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
    ```

- nvm.sh 등록

  - $ `. ~/.nvm/nvm.sh`
    - 첫번째 `.` -> source 명령어를 의미 (환경 변수를 변경할 때 사용)
    - 두번째 `.` -> 숨김 파일/디렉터리
    - 세번째 `.` -> 파일명과 확장자를 구분하는 구분자

- 10.13.0 버전의 node.js를 설치

  - $ `nvm install 10.13.0`

    ```
    Downloading and installing node v10.13.0...
    Downloading https://nodejs.org/dist/v10.13.0/node-v10.13.0-linux-x64.tar.xz...
    ############################################################################################# 100.0%
    Computing checksum with sha256sum
    Checksums matched!
    Now using node v10.13.0 (npm v6.4.1)
    Creating default alias: default -> 10.13.0 (-> v10.13.0)
    ```

- node.js 설치 확인

  - $ `node -e "console.log('Running Node.js ' + process.version)"`

    ```
    Running Node.js v10.13.0
    ```

    