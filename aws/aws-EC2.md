## EC2 (Amazon Elastic Compute Cloud)

> 물리 서버가 하는 역할을 복제한 클라우드 서비스

## LAB1 : Create a Windows EC2 Instance and Connect using Remote Desktop Protocol (RDP)

![img](aws-EC2.assets/-dfF5wVQSb1YlpZm0RJ2xveO1J4xWp59Xyxjw8E43lF7EQCP-PRG7ou9SZAGTkeWq8_XSw3JJJXoe-QbrlzSiuUOKGsMKg5sbzYrZoiYY7rGkxJnodViZ6LYF0r10TFo92ulrLnv) ![img](aws-EC2.assets/tITIBaF1qcSJsCnaOPmMTvCe6o_xxm4QKxr2INyJRirRpydNLzxKiSHnWIreJcuFLRSs15w-zBQ0cNUkTacDAO6axDU4EbIN96DJoS8eIwnSVuluTPdnpl-TJYJtCw5hGCfaXoBy)

- 일반적으로

  - `Linux` 기반의 인스턴스의 경우에는 SSH를 이용해서 연결

  - `Windows` EC2 인스턴스의 경우에는 일반적으로 RDP(원격 데스크탑 프로토콜)를 사용해서 연결

    - `RDP` = Windows 컴퓨터를 관리하기 위한 GUI

  - 공통적으로

    1. 퍼블릭 IP
    2. 외부에서 접속할 수 있도록 서비스 포트를 개방 (보안그룹, NACL)
    3. 인터넷 게이트웨이로 라우팅이 설정
    4. 개인키 파일을 가지고 있어야함

    

- VPC 서비스를 확인

  - 라우팅 테이블 
    - IGW와 연결을 확인
  - NACL
    - RDP 접속을 허용
  - SG
    - 모든 접속을 허용

- RDP 전용 보안 그룹을 생성

![img](aws-EC2.assets/ANggTUODqyMTvhI-x3og2PU1Lc-4FFCn9Qk8JxPhKHUcFqs79EY5oN6tnrYYot3R1MqwjloK7tzWJaFtV-G3jdEyXgEZe_doQzJdAku6joNwKWQRnRiK_tuUam5IQOPIUgQfSHyA)

![img](aws-EC2.assets/JFKedP5F8MRJvNHLWFJlkklTm1AAiGD2RvAo95x3gOf-WxjsObWvGrC6MehQsbRlVcTvO-n874h6mQVy8yhVu6AsDkP6iybxVIcvjWLCA46-1X0INQuRF3mxJAp5HlWZokKRxVyi)

- Windows EC2 인스턴스를 생성

![img](aws-EC2.assets/C3fNJr775S6ElMBn6XxYCOO0O80kqc5Oa4NeIxef9jhmf0zYltYuhmQ0xKYKkDAlUf5xjQYSpf5ThN79xvabkyKUNNygUeW5RSvWiKHeFZuJ5imvKvREYeZ38nrFGNZ8BFCMNYOp)

![img](aws-EC2.assets/uwc9qF-htP8E6oAPb8E8M9-vWprdiDmm0urGGHoXhZljl_THXR7NSjkPGvIrD9WP6lDp37E9gLVvX72CxyZElLx_pcyYHMmCTkJbyZRHqvb6LSUNaMn9RfGNtZ2G7wRnzKLzG_G-)

![img](aws-EC2.assets/t7i8vu0piveyRRBQq8gRQl2EyWg0Zxufia7jrxm_6mNv7oyWAonUB3la2E9ojMnYVROC3SQLGaU7UwGKDz8RjzS-GZ7-pCwS5hGInc84smIOmP83Ml-gcjfb06Lsh4ktfZAlg-_k)

![img](aws-EC2.assets/-EvllXmzWBI5sOCSxUx9xjaaK83J3b4KBjoNoyPrFrQSqz8xCK-AtCbJeSPmv37vc1i6USahFA_ufFrAtiTIu3inD10miQxmmn_i2cHH_cNMTUH4Y0nRuRcUSL7JQ6Y1Wr8DqstE)

![img](aws-EC2.assets/ZwWhEbIZW1etyfItn4dUYNHaRDiJxV2VCZ6F1dokcPCn01IAIXverZFFdKlhM0cpIpZGy8K2FKSod2hOPgrvyd9E5OPAkQo-Bc7Y30YcqxTolwXJXg7Ac8n8_iFzHOHuPpvbzHRL)

![img](aws-EC2.assets/8FNis86ZEL_rU9Cgopr_Jzly74enf-SIOXsmlMi_ft16hF2pFT6l2iL9fqDQ-Bn1RaCksHEoxNxIfOBI7HSdkBY3egxB6pI1THhsOLwLgB-6uy2s9oKUOeUkZKS7ZFIv_uU2PUfB)

- 약 4분 정도 지난 후 (3) 암호 가져오기 링크를 클릭

![img](aws-EC2.assets/9pJZq-2fhHA8R8KXXd5_soJMm3BhNap6p203R2jPkyR8I8YUMFqopU7f2IkrO3LGW_i_AS9dvxjBRIM-eIknucUaa3L_4NvP3KcXchrsh1J0oBcuvK1GH9PN2fkV15fGkk69Yitd)

- (4)  Browser 버튼을 클릭 후 인스턴스 생성 시 다운로드 받은 개인 키 파일(windowsrdp.perm)을 선택



![img](aws-EC2.assets/DxXkXp5R0PSHr-LNBjzLA298TYsG_Dt46XwuKN6D0LGAMO0myDIZGfh7CKZ3cahDy8Y5S7EN1DuS8FzF4YCpY3GJY7arP5ldj6mhky2f__xLBa9N_hF_NWWgHXsvnlQBHHNq-axC)

- (6) 접속 패스워드를 확인
- (3) 에서 다운로드 받은 원격 데스크탑 파일을 실행

![img](aws-EC2.assets/Sn0c_f509HGdqC5veTvc8Tpg3S1thtr6aIQJLdxXc2qOTK8Q22WOgd2jDo-b07dPnxoW3aVJ97-14wZllbUEbEcXCH6c5C4Dh0t0q0d_jl52YZajsQ_QE7SiIvrQD6BYaOk3tVur)

(6)에서 생성한 패스워드로 원격 접속

![img](aws-EC2.assets/lwnaTUQRhbs2HTnGoJUBbrvwf1Su7o0IUANh1tgqsbMJizQYeak1Zf3KhV9TIIh0veUTKJzOV3dHSiDuG_mPaUyEyfKBPzxTCmRt6eYzzexcvp__mDzK6X_hezhq6v7OwSKtGH2z)

<br/>

---

<br/>

## LAB2 : Creating Your Own EC2 Workstation in the AWS Console

> EC2 인스턴스 생성 후 해당 인스턴스를 통해 S3 버킷을 생성하고, SNS 문자 메시지 전송

- Workstation
  - 작업을 위한 EC2 인스턴스
  - AWS CLI로 AWS 인스턴스를 생성하고 관리할 수 있는 권한을 가진 인스턴스

![img](aws-EC2.assets/Pn_3N7iWBc9tQ4urn6X0-xX38wOXhhMgxOJYojPbDuCrRPnSNJ43sndxBlv9IIjP05U8A1a3jFIoDmI98pc6abAhGZ1P5Y_eYUTjboJ4uKdwA6-NDTIb7iTdp3vJG6TJuGKCoX_q)

### 목표 : EC2 인스턴스 생성 후 해당 인스턴스를 통해 S3 버킷을 생성하고, SNS 문자 메시지 전송

### 1. EC2 인스턴스를 생성

![img](aws-EC2.assets/_sN4P8BoqiBEeP9a2I-xb9LxtFYpjVfhCvYttTGdHUymfKiyoK7m0go4GDKNsBmADzy0hV1QFcS9zjIoOvHTE9_gcq9Z2c8oK2a_k_u2Ov89mBsFphHsH1D07Krd2qX5SzjR8Kac)

![img](aws-EC2.assets/0syDyw18yTgE-1YNNpUiHPk_RlgWHpkex1XnQZ3762mJVAUzJGnKALHPWctd6N0ATavWT73ifm3-J2C-7oojcoDkpY0ohBg7xJ-1_PK2zLzbsmX396-eGdZ4Ml2wB6u22fcI_1ul)

![img](aws-EC2.assets/O804fPfEDeLcXt2sKPY2Eslf0DCRmRqXFcUNIr4nMr_Uwp1G4jEtWbrOK8bibdW8I7aV3s5HAGh23q2ibB7WXb7IJQv8h-WXe2DFeV6-BfUNExLJJPEWw01nIuG7poMJ3N5n8fXN)

- EC2InstanceCDALabRole 
  - EC2 인스턴스에서 AWS 내의 다양한 작업을 수행할 수 있는 권한을 부여

### 2.  SSH 클라이언트를 이용해서 EC2 인스턴스에 접속

![img](aws-EC2.assets/eGpNdafJB-tIuzeNNN6owxuGkbnJhZit-NjMQWr9GmLsjQGbMH_M0kVm1iPMhbopQqLu3SrKe5we05EQXEbasq29_3Pp0P_AesksfRBHF1USmqtKzQpe90PVs6BiE9NIFQl-8leg)

- 인스턴스의 퍼블릭 IP 주소를 확인

![img](aws-EC2.assets/OyTlMxQWRWAuAC6AroHjKUTx75S_IqmV1MIcOJY8O3gWg1E9e_Bw6syAbclSI3UWHkcWV8Z_0phXpeptJ6TAJIChLZCNGTWlmeMdntigdC8-rChtS2rovJdJHUWxTVRXC48dSkHX)

- 인스턴스 생성 시 발급받은 개인키를 등록

![img](aws-EC2.assets/NZ_y5kcXNQAU4q-h-47OLjVFYydXnSmSnKcc4A0r2uH_gf-jQsuh1zXHue57zFYOMA9o_iPznsOgI9ZwdrYno79CSb-PpGa_ns-p7dOsMmrqx5bW2E6rWkEshZiWvpttAsNTGhdY)

- ec2-user 계정으로 접속

<br/>

### 3 .SSH 접속 후

#### 1. aws config 명령어로 기본 지역(region)을 us-east-1로 설정

- `aws dynamodb list-tables`

```
[ec2-user@ip-10-0-0-63 ~]$ aws dynamodb list-tables
You must specify a region. You can also configure your region by running "aws configure".
```

- `aws configure`

```
[ec2-user@ip-10-0-0-63 ~]$ aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: us-east-1
Default output format [None]:
```

#### 2. DynamoDB에 테이블 목록을 조회

- `aws dynamodb list-tables`

```
{
    "TableNames": []
}
```

- DynamoDB CLI 명령어
  -  ⇒ https://docs.aws.amazon.com/cli/latest/reference/dynamodb/index.html
  -  ⇒ https://docs.aws.amazon.com/cli/latest/reference/dynamodb/list-tables.html 		

#### 3.  S3 생성 및 조회

- 버킷 조회
  - `aws s3 ls`
- 버킷 생성
  - `aws s3 mb s3://1yangsh-20210302`
  - 버킷 이름은 유일하게!!
- `aws s3 ls`

```
2021-03-02 05:37:24 1yangsh-20210302   ⇐ 생성한 버킷이 조회
```

#### 4. boto3(Python API) SDK를 이용한 SNS 서비스

- https://boto3.amazonaws.com/v1/documentation/api/latest/index.html
- https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sns.html#SNS.Client.publish
- pip 설치
  - `sudo yum -y install python-pip`
- boto3 설치
  - `sudo pip install boto3`
- `python`

```python
>>> import boto3
>>> sns = boto3.client('sns')
>>> phone_number = '+821050591152'
>>> sns.publish(Message='ysh410@gmail.com', PhoneNumber=phone_number)
```

![image-20210302145054935](aws-EC2.assets/image-20210302145054935.png)

#### 기타 : PC에 AWS CLI 다운로드 및 설치

- Windows
  
- https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-windows.html
  
- MacOS

  - https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-mac.html

  <br/>

---

<br/>

## LAB3 : Create a Custom AMI in AWS

> 가상머신 이미지 생성

- 링크
  - https://learn.acloud.guru/course/178db59b-70f1-4bd8-8d74-9ab9263f8f9a/learn/c346e351-30dd-4668-ac6c-be9cdb4e7de5/c01f3c88-73a6-4cd3-9674-6008cc45ff95/lab/1ca2ca4a-48bc-49c7-b3df-e583352e8b0a
- 가상머신 이미지 생성 케이스
  1. Auto Scaling 환경과 같이 동일한 상태의 어플리케이션이 즉시 사용해야 하는 경우 
     - → 사용자 지정 AMI를 생성해서 사전 구성된 인스턴스를 시작하고 대리를 건너 뛸 수 있음

#### 1. 인스턴스 생성

#### 2. 인스턴스 생성 후 SSH 접속, 아래의 명령어를 실행

> apache 와 php7 설치

- `sudo yum update -y`
- `sudo yum install -y http php`
- `sudo service httpd start`
- `sudo chkconfig httpd on`
- `sudo usermod -a -G ec2-user`
- `sudo chown -R ec2-user:apache /var/www`
- `echo "<?php phpinfo(); ?>" > /var/ww/html/phpinfo.php`

#### 3. 사용자 정의 AMI 생성

- 현재 인스턴스의 상태를 사용자 정의 AMI로 생성

![image-20210302162504107](aws-EC2.assets/image-20210302162504107.png)

#### 추가 : 사용자 정의 AMI를 이용해서 인스턴스를 만들 때

- 이미지를 만들 때와 동일한 리전에서만 인스턴스 생성이 가능만약, 다른 리전에 인스턴를 생성할 경우에는 AMI 이미지를 복사해서 사용

![img](aws-EC2.assets/DDB5_N572mUgDsX7aR3JWjTEOOgwMSQ_QxttoY7TLCbA896X08SyB3Y19MXBd3uc-AqXvqfNDRXhBkZoizeBIUiWR1NoNTyiiklt3ntAh0zUMG3b8NlaQWk87ekIl7esUe8NEArb)

![img](aws-EC2.assets/vzKkEBFhB-jofvBzcFDXmY5qWcDENJ-HcLZhKHomQf0_ERjgGK7toyMQn8biK52o2KnfmyGFnpfF_kNrFo_uKLREDS7QKxEVw_C0yAU9Ty9LcKQTd3BjAJ2wvFBL7i-gpKedxlmI)

- 사용자 정의 AMI를 이용해서 인스턴스를 생성
  - ⇒ 응용 프로그램 설치 및 설정 시간을 단축할 수 있음

<br/>

---

<br/>

## LAB4 : Resizing Root AWS EBS Volumes to Increase Performance

> 루트 볼륨의 크기를 조정 ⇐ IOPS(Input/Output Operations Per Second: 아이옵스) 향상을 위해서

1. 독립형 인스턴스 (bastion host)

2. Auto Scaling Group (2개의 웹 서버 인스턴스)



![img](aws-EC2.assets/6zwMHWBmcb9uxrgSi0ng0OdDbwPcQjIHt-edWD1wHrcboFdsVds9OOxTrGdNepNO6w_L113Mj-wBKUSJRW7Nfj99iZfxDd-_nw4ZkKcjGwBwh67gARXfe6ytUrOZ3U-8AbWlwwam)

### 독립형 인스턴스 (bastion host)

#### 1. EBS 스냅샷 생성

![img](aws-EC2.assets/Sg3LMRZ7opu060b1Gabpyhh0KMCNXNLtZDVkEJHO5fwjShA3tWHo6difOZ-nAi6cP_BVm0RPgqecLC15hHaUjqGiZwp4U7LRSs0ZmFjChsdslFZBAqlUv457snzPFCD6Jz1aq2ua)

#### 2. 루트 디바이스의 볼륨 ID 링크를 클릭 후 작업에서 스냅샷 생성을 선택

![img](aws-EC2.assets/1dg1ofZs9yn6LTJlRtRiesh68GyXMCzEolgHNSErh-wMIJtJivcTaVO1EYid-Nen5-koNMQkFhOE_Nkvqnx8DEwE0gC_DbYRX7oEgyMcYQq51QppZmlLW_V5kzTh1gvhwmJyPd_Y)

![img](aws-EC2.assets/cWRfzxIo9H95ZHUPPEvPqGXlgHjAFWNFMGWXUl-WJFLkGz0xdaUBwUWdvnmtkSQBb968lIm_RjAG6oIVj_zD6cEMDWmCrt9Eig16YrIj1SamcTZF8JBKbbm0X0Ai0cQKJ8zB0QJI)

#### 3. 더 큰 크기의 EBS 볼륨을 생성

![img](aws-EC2.assets/6933Bw3YkKV-vGTKgSQmgm6y4ld-xCvO10qWkrw8plgjVY7rGuNYV9PNHpX8UcGRsr7LnGLcJHXImFMDXnQMb_xV_4KsQDJhFi-WkVWNBrzeaJYMDRtaf3cDKg2ADhV-4vsbJHnf)

![img](aws-EC2.assets/tz07XK_a8GVNdoACsZbh9iVvIFA8hAI5xoTmmLTLHItBSPmYM35lkNk5KoWE1W78R0jaJC3FhGjzMnZaT82TgJV_aj-WT7uMxv0myQns5ZQ34l_ZctqWMa2BxXLfMAzIJg_Ec9no)

![img](aws-EC2.assets/TGPbT2z4sclVt1OWwDXvR9pgctElrqBPSEkqnQOthiztlng2jg2h1ELd9uU3amwIo9Pf3P8l3CIDo8z8HpAKxv_DH3zF2s8kRoo8IVOyR-kc_xrq1ayHLVFC5swhVFwKwK4gRdJR)



#### 4. 생성한 볼륨을 EC2 instance에 적용

1. 적용할 인스턴스 중지

![img](aws-EC2.assets/QkP2vlVr8NN6BK6AwKPIR4_SjJSZMhfsne3E6DYJ5NudnP0e45zbwXcRB-KcUkqoYNKa9cDyjsRaz2-qPC-TTMfg2dMQxLqnXdVp5gQ_lzeoIdhttrgkiOTVviGlFWn-lfBmUkUh)

2. bastion host 볼륨 선택

   ![img](aws-EC2.assets/nQYqfz0vdydoGDTFFeyXDMGhE3l0Y2FaAwOyrJ2fqGx8DUFtCpwlbmY0RWJRqVXy1T95wpokR7BXXLgqCCWEP-W8RYKFsotG_ivcmylUNolyVHgjiNGsnMlZuwBTip4elahUYz-K)

3. 볼륨 분리

![img](aws-EC2.assets/KmV7BnxIKL7y31FSHs_tCQgJktSnlcPQ7Tu56PUvYPlU-0GIqwOZ_VjrK0whETVRrRdnB3DGqWhZnt1XzvxfAUbYyQYraOyXSU4NYClZvcCmQPZi06A_aAtrXsI7M5qiHDih3tW-)

4. 볼륨 연결

![img](aws-EC2.assets/-KoPAox8LhC7zRZnVu9bI_CXgqKGxr1aHzeET8zSND30_1ScUMeiA7LAZ5OTxYxZiDlqsnlEjtU1yhgzCc_VLgEkZxdn65m7W4wVuJEqwQDDdxSs4TLP1UyJhsZ7GQjXF8jqOt50)

![img](aws-EC2.assets/D2YJtxo4RkTZ3twEHaWvfU-ZvshJPcj0_cByc4_n2i-H79TOyiaJBy0GaKIx-n-bILWIJ4ULL7Kv9fszeabWqcZDo1KxoeX5_3_n9-HBGwKGHhLOq5cCyng3H2StQQ9YdVSmdg1g)

5. 중지된 인스턴스를 시작

![img](aws-EC2.assets/h8znw5tTMXvWafDb44DE8BkHcYQG-ZTnNA9uzyoPPjvl6LUK-11q25FRaJ3NGD8EuM9Q30uf-F_D-zrdbHs6shr6M3IR6mWOLKq-ZHfpm8cX1ET9TeSD1vkCWK9KNuN1myvaGsqD)

6. 스토리지 확인

![img](aws-EC2.assets/EJ_CcM5vCRWYxX0Ij5PUy4S7Nl99si7_7eje0RT7aYZU1VhrWkOYTxc0_2O1xJc9xLmsaYZhXhU89e0Pf4AA2G6h7p-XBPFnqty6y3hOGInc1uAdS3jmQHQvreW1lfpA05tkyKEl)

7. SSH 접속해서 확인

```
[cloud_user@ip-10-99-1-27 ~]$ lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  40G  0 disk		           	⇐ 확인
└─xvda1 202:1    0  40G  0 part /
```

---

<br/>

### Auto Scaling Group 설정

- 인스턴스를 확인

![img](aws-EC2.assets/fiH_bh6hsEOpnYCmLCUkSUGibCuc00vErBlbe6CfdXVN-cJe9XdqmCQekSMfZ3HYhZmosJXZycpKW9J-tfF26rXIePHsRgu3gAht75MyngEqD8UF2rtlqZEJNuMJjFXwVKWeZ7Iy)

- Auto Scaling 확인

![img](aws-EC2.assets/p2ZgVxPx8WVNn-dXBE5naD-w8Yn201sxQduXSwGh6wtojCWg-OX8wmYcGeV5ECRelILzjLO-mrBtvHIZ9eWUumonfu8qZTlFGu8o6sAwEYd6Xl-_htNRKFkDkxb3cNwdoMkRLmAw)

- 시작 구성을 확인

![img](aws-EC2.assets/DsmUcib8Kd9ke84V_nJY51EVaxNGAJoKqENSUR_XMIqPMMBfP6CgwiLzYplqPJkKVtbE2Y77b9ZkZMkePs9ejfXzArn8VRGrR5Y9P1Bsi5jtIuKN_ikea-bmfxRJWS8LLsPjM4JJ)

![img](aws-EC2.assets/uVum1wSKcqBCpkoogu-yAJA51UCmeMkIXvsSuG03--FZrHbky8RnsYncaMMZnF6nJGf5cgS2p-veKSFm-c0wgQsSlBjquB57yQ1BCWK0y3hUTFTNx98Fkt9OIPqs65Dvw7_I1Dv8)

- 새로운 시작 구성을 생성

![img](aws-EC2.assets/Azji74iopTISmCbEOepKh6InX5yuvTtnOCZZWoiMNostZM-0nEQu1hYY56vUL6690wb-CTn0UnB0xja23j_AnyLGXeMxTiDK7N1hLer4oKbQQFH8kqvWiNJpSyZe82YAsroFJbGU)

![img](aws-EC2.assets/HNy7DD9um5rgUl1BfYDD_mzX-GGhDxnc0tr-VjfSEN1godkzZCqBTpG1g3fffLYdEUgTmzFwQIwelGc8T9XMnebGRJYQoAh6_BVfRLPggkYDM4u82uqmaFJJrxasCROk_h2sEw__)

![img](aws-EC2.assets/3Hq6swXs-l2xYaLc4ehG-dk5_GivcngZzEv17YtH9iAiAlFp8t_UitXDfvC0KQs412zLquhaMPT1yCKE31lI6jM9IuyUxp-sQqTWgCOu47dAIGIcfElePtcnvbp1DJW_jzUdyLxb)

![img](aws-EC2.assets/JmaOtVUH9SZUyjkuT877Hx3d0XvUnv1a9eWFb4gCjFs76KlWBrmbAH_uqauDdpE8-RSerjDxT57mtXu0AEr5hfqCn_1e6BbCvexNqBCt6OO0EmmqzErBexDfOzvloBwzrO2mt9cV)

- Auto Scaling Group에 새롭게 생성한 시작 구성으로 변경

![img](aws-EC2.assets/daW1VWUj0WU0BIjWNR01P2DyS8cPdYYtabAshCdJIS2-nl_vpODDOrJOZufl_EEfarR3yvcP8_QVZXxoYfBYY_byHOrGKI1Gpd-ToX-TSvq6KRfOjHHQibsrsD6TSmxYhIX8BRnt)

- 기존에 동작하고 있던 인스턴스를 종료 →  Auto Scaling 설정에 따라 (시작 구성에 설정된) 새로운 인스턴스가 생성

![img](aws-EC2.assets/zL0YSmJ0IiPKe1_hkTjfAofkzXEBpzRr04MOf4A01d3Em_obNZUOeW6koV1xcGAwLxk2iajuLNz7YLGLu5hlQe-8xi4cE_uLYj0myRGHkx6Nh8Ik2cdPDh3sKMEBl7h9LhdWPoob)

![img](aws-EC2.assets/0-hNKbI14IL4-uFj1-UDgilKvPSQyMzkd55IfhD6tfTuNBF2JhJlaTK4GDRjg0U2jEvtiBFoZoFLPqtX9itgUYHAFG2tpNUl9ll0ZMqIavVJs_-R1_Ru4DpGgFC7rutf8oeyOsh_)