## Cloud Fundamentals

> chapter2

<br/>



### 가상서버

- 대표적으로 Amazon의 `EC2`

- AWS의 가상 서버 이용 흐름

1. 가상 서버 생성할 리전 선택
2. 접속을 위한 엑세스 키 생성
3. 통신 유형 설정
4. 이용하는 OS와 루트 디스크를 선택
5. 사용한 가상 서버와 스펙 선택
6. 이용하는 가상 서버의 상세 설정
7. 가상 서버에 추가 연결한 디스크 선택



- 리전 선택

- 단계 1 : AMI 선택 (OS에 맞는 이미지)
- 단계 2 : 인스턴스 유형 선택
- 단계 3 : 인스턴스 세부 정보 구성
- 단계 4 : 스토리지 추가
- 단계 5 : 태그 추가
- 단계 6 : 보안 그룹 구성
- 단계 7 : 검토



- server 접속(Shell 이용)

```
ssh -i .\<key 파일> ec2-user@<ip주소>
```





---

<br/>

#### 가상 서버에서 사용할 수 있는 옵션 기능

- 시스템의 이중화 및 부하 분산에는 로드 밸런서의 기능을 사용
  - 로드 밸런서 기능으로 서버의 접속량을 나눈다
- 오토 스케일 기능으로 가상 서버의 대수를 자동적으로 증감
- 스냅샷(백업기능)으로 가상 서버 디스크를 자동 백업

<br/>



#### 클라우드 스토리지 서비스

- 데이터 아카이브, 백업, 파일 서버, 재해 대책 등의 용도로 이용되는 대표적인 서비스
- 스토리지 서비스의 예로 AWS의 `S3`, `Glacier`는 데이터를 오래 보관하기 위한 비용을 줄인 서비스

<br/>



#### 클라우드 네트워크 서비스

- 클라우드 위에 네트워크를 구축
- 네트워크 기능을 가진 클라우드 서비스의 예로` Amazon VPC(Virtual PrivateCloud)`
- 보안 기능으로 필터링을 통해 불필요한 통신을 차단

<br/>



#### 클라우드 데이터베이스 서비스

- AWS의 `Amazon RDS`는 일반적인 RDBMS이다. 

  ```
  NoSQL (<-> RDB)
  - 검색속도 낮음 / 저장속도 높음
  - MongoDB
  - Casandra
  - DynamoDB
  ```

<br/>



#### 클라우드 데이터 분석 서비스

- GCP의 주요 데이터 분석 서비스
  - BigQuery
  - Cloud DataFlow
    - 일괄처리
  - Cloud Dataproc
    - Spark와 Hadoop 클러스터를 간편하게 이용
      - Hadoop = HDFS + Map Reduce
  - Cloud Datalab
  - Cloud Dataprep

<br/>



#### 클라우드 AI/기계학습 서비스

| Amazon Web Service(AWS) | Microsoft Azure  | Google Cloud Platform(GCP)    |
| ----------------------- | ---------------- | ----------------------------- |
| SageMaker               | Machine Learning | Cloud Machine Learning Engine |

<br/>



#### 클라우드를 이용한 시스템 구축의 대표적인 사고방식

- 로드 밸런서와 오토 스케일 기능으로 시스템을 확장할 수 있도록 만든다

- 하나의 존에서 장애가 발생하더라도 문제가 없도록 멀티 존으로 구성 (이중화, 가용성)

- 서버 등의 다운을 모니터링하고 자동으로 복구하도록 구성한다 

- 시스템을 자동으로 복사하여, 백업할 수 있도록 한다. (Snapshot)

- API를 통해 다른 서비스와 연계시키는 환경을 유지한다

- 온프레미스 및 기타 클라우드 서비스로 이전할 가능성도 고려한다.

  

