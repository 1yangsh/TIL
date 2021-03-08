## AWS

> Amazon Web Service

### 공부 순서

**1. 서비스를 이해하고 사용방법을 연습**

- <a href=https://acloudguru.com/>https://acloudguru.com/</a>
- Learn AWS by Doing 과정

**2. 교재**

- 서비스 운영이 쉬워지는 AWS 인프라 구축
  - EC2 기반의 서비스 인프라 구축

**3. 서버리스 아키텍처**

- 교재 - AWS 기반 서버리스 아키텍처 
- node.js <= javascript에 대한 이해가 필요

---

### 클라우드 특징

1. 확장성
   - 파일 형태로 서버를 구축하기 때문에 복제, 삭제 용이
   - 외부에 요청에 따라 서버를 확장, 축소가 용이 (Auto Scaling)
2. 탄력성
   - 수요가 떨어졌을 때 용량을 자동으로 줄이는 개념 (비용 효율성)
3. 비용관리
   - 자본지출(CAPEX)에서 운영비용(OPEX)으로 IT 지출 내용을 변경
     - 장기적 수요를 예측
     - 위험을 감수

---

### AWS 클라우드 서비스 범위

- 컴퓨팅
  - 물리 서버가 하는 역할을 복제한 클라우드 서비스
    - Auto Scaling, Load Balancing, Serverless
  - 대표적으로 `EC2(Elastic Compute Cloud)`
  - Lambda
  - Auto Scaling
  - Elastic Load Balancing
  - Elastic Beanstalk
- 네트워킹
  - 어플리케이션 연결, 접근 제어, 원접 연결
  - `VPC(Virtual Private Cloud)`
  - Direct Connect
  - Route 52 (DNS 서비스)
  - CloudFront
- 스토리지
  - 빠른 액세스, 장기 백업과 같은 요구를 충족하는 스토리지 플랫폼
  - `S3(Simple Storage Service)`
  - Glacier
  - EBS(Elastic Block Store)
  - Storage Gateway
- 데이터베이스
  - 관계형, NoSQL, 캐싱 등 
  - `RDS(Relational Database Service)`
  - DynamoDB
- 어플리케이션 관리
  - CloudWatch
  - CloudFormation
  - CloudTrail
  - Config
- 보안과 자격 증명
  - `IAM(Identity and Access Management)`
  - KMS(Key Management Service)
  - Directory Service
- 어플리케이션 통합
  - SNS(Simple Notification Service)
  - SWF(Simple WorkFlow)
  - SQS(Simple Queue Service)
  - API Gateway

---



