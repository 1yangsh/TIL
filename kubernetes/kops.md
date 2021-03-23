## Kops

> 구글의 GKE, Amazon EKS 등 클라우드 환경에서의 Kubernetes 설치 프로그램 
> 구글에서 개발

## 1. 작업 환경 구축
- 가상머신
	- Ubuntu 18.04 준비 (t2.micro) -> US 리전 (상대적으로 저렴)
    



- ssh 접속 (로컬 shell로 접속하는 방법)
  1. key pair 다운로드한 디렉터리로 이동
     - `ssh -i <key값> <user-name>@<public ip>`
  2. 연결이 안되는 경우
     - 홈 경로로 이동
       -  `cd ~`
     - .ssh 로 이동
       - `cd .ssh`
       - `known_host` 의 내용을 전부 지운 후 다시 접속



- kops 설치
  -  $ `wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64`
  -  $ `chmod +x ./kops`
  -  $ `sudo mv ./kops /usr/local/bin/kops`



- kuberctl 설치
  - $  `wget -O kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl`
  - $ `chmod +x ./kubectl`
  - $ `sudo mv ./kubectl /usr/local/bin/kubectl`



- IAM – Group 생성 → User 생성
  - AmazonEC2FullAccess
  - AmazonRoute53FullAccess
  - AmazonS3FullAccess
  - IAMFullAccess
  - AmazonVPCFullAccess



- AWS CLI 설치
  - $ `sudo apt update`
  - $ `sudo apt install -y python3-pip`
  - $ `pip3 install awscli`



- AWS CLI 설정

  - $ `aws configure`

    ```
    AWS Access Key ID [None]: <Your access key id>
    AWS Secret Access Key [None]: <Your secret access key>
    Default region name [None]: ap-northeast-2 (or es-east-1)
    Default output format [None]:
    ```

  - $ `aws ec2 describe-instances`

  - $ `aws iam list-users`





## 클러스터 생성

- S3 버킷 생성

  - ````
    $ aws s3api create-bucket \
    --bucket <bucket name> \
    --region ap-northeast-2 \
    --create-bucket-configuration LocationConstraint=ap-northeast-2
    ````

  - ```
    $ aws s3api put-bucket-versioning \
    --bucket <bucket name> \
    --versioning-configuration Status=Enabled
    ```



- 환경 변수 설정
  - $ `export AWS_ACCESS_KEY_ID=$(aws configure get aws_access_key_id)`
  - $ `export AWS_SECRET_ACCESS_KEY=$(aws configure get aws_secret_access_key)`
  -  $ `export NAME=<cluster name>`
  - $ `export KOPS_STATE_STORE=s3://<bucket name>`



- SSH Key Pair 생성
  - $ `ssh-keygen –t rsa`



- 사용 가능한  AZ 확인
  - $ `aws ec2 describe-availability-zones --region ap-northeast-2`



- 클러스터 생성을 위한 AZ 지정
  -  $ `kops create cluster --zones ap-northeast-2c ${NAME}`
  -  $ `kops edit cluster ${NAME}`
  -  $ `kops get ig --name ${NAME}`



- 마스터 노드 확인, 노드 수를 조절
  - $ `kops edit ig master-ap-northeast-2c --name ${NAME}`
  - $ `kops edit ig nodes --name ${NAME}`



- 클러스터 생성
  -  $ `kops update cluster ${NAME} --yes`



- 클러스터 테스트
  - 클러스터 상태 확인
    - $ `kops validate cluster`
  - 노드 목록 가져오기
    - $ `kubectl get nodes --show-labels`
  - kube-system 네임스페이스 안의 pod 목록
    - $ `kubectl -n kube-system get po`



- 클러스터 삭제 
  -  $ `kops delete cluster --name ${NAME} --yes`

