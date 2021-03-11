## 개발 환경 구성 

> configuration of development environment

```
참고 : AWS 기반 서버리스 아키텍처
```

### #1 AWS CLI 설치 확인

- `aws --version`
  - 설치 :  https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2.html

### #2 node.js 설치 확인

- `node --version`
- `npm --version`
  - 설치 : https://nodejs.org/ko/

### #3 IAM에서 사용자 생성 (p322)

- IAM 사용자 추가

  ![img](development-environment.assets/q_wLKPXJFsdnJPoLSZUGYXSQCBZ_x7eOZ6rJaJnta5u8dnohxCNjHmCCZpZSfqZwhF86dRW3_uGL-eVfSu7K_4JqzY1FTDJRfdtUtE6Y9zmydr2UDhxN7wVgzLOxqB_5TNhY2jHh)

  ![img](development-environment.assets/dkIbFXtrp_qlUBsF-ljybJtVRwPTUNWifINUDGxWFamIAukRVS01MDD-AOT8-SxlXnnbdBjRRhYz2UT79Ae1dmBLTlxVlFuHOq7KSYmamXya1DMdkJxaPqe0RokFOsh_Pb4EJpNY)

  ![img](development-environment.assets/KYDtwpuVp3geMASx74z_SiRT9au1hrfu2d4CQRm7vHrTR_Gb1nEA7CqEtHpVsEQCWBhlZ0UuFKeY2H2IQhlJe3TCwy5QUKX-MxdKfrbtgnZW7idrmJnO85PBgTH99_0EuSidlv-_)





### #4 CLI 환경에서 작업할 수 있도록 액세스 키 ID, 비밀 엑세스 키, 리전 정보를 설정

- `aws configure`

  ```
  AWS Access Key ID [****************3EGR]: AKIAS4**********4OWB
  AWS Secret Access Key [****************9fk/]: d91uKydp/9***************rLI8zSPBXaZtVx2
  Default region name [us-east-1]: us-east-1			⇐ 버지니아 북부 
  Default output format [None]:
  ```



### #5 사용자 권한 설정

- IAM 사용자 -> 권한

  ![img](development-environment.assets/JbZnD7_DqSZGEZP_QSdtpcUQdKmaK8oiQAyYdAqPh8xGC-IB_CjI7lutPfGuTM52CyzDRCAHm_Z1PI_nXR7YXNVPW4DL7l5XVb-vFjHpUUKwngcb8cQ0nHSDFu_rPkuytnuktaUL)

  ![img](development-environment.assets/qaNnEn22dPmON6Ql0vAhOh3clopUDDStIWyko4fdFrROdszf9ujBQig1bNh4eXuYMBExoEeKB1kKRFfarmUkwEnrTAjCNvZiQy-Jjul6Bk4pr-WZEypVQlXpCy2oDJenho9g8N1a)

  ![img](development-environment.assets/K5ImbxvqFKkeY95go4Gu5-2fy8_hqOeqnLAH_J2IWcwvLx01EIXYpg0y7jh_f8jWqf81rqzjBdi22ze7yP3VdWGj8JKxS8gp0vfgmX7IaPiVH12TVl_jvt6hExnu9IimvNTf8NyJ)

  ![img](development-environment.assets/XAFfGu7OVWUIKSXxWvFMBG3GJdZ40YgNc5OluToHz5EsQ3a9GE77SlQdZgzItdlFMsPtOD2TbP81djA8LhLV_9RlGy-FKmK3ql7gkly51upqXeAc-0CM5BezalHiRW6U8xNPIi3u)



### #6 S3 버킷 생성 (p326)

- 동영상을 업로드할 버킷과 Elastic Transcoder로 트랜스코딩된 동영상을 저장할 버킷을 각각 생성

  ![img](development-environment.assets/hXuKR-NC-9uysEkbvFIFfVU-UF1T7K4GQ9eHGo_KWoYylneTlPKFXtnUNUmtSIJr38Qagn01MaQHhyzDTHCx4n6Lw0RkbWa2_SXwdsUUTA2bUv9aeXT4tnCp2NZQH7o5Rrk1Itq3)

  ![img](development-environment.assets/s79X4DJEwD1k3jTLYYcT_qaO_eT8OrFl6PppIphqYnaRY-UjQ2xeO3ocPMUcTaZ9JBsr6FW6hvzZSTddjnOK2Wbp5yF4itLfNvqy-7nOjKVph00DBUjKVeE5BqXoMW2Qh6GtZ6Ku)

  ![img](development-environment.assets/oRwIaeYHdgaIO93LclPxvwOLhAzNy2LQaIMBPGmat6iBulg6D36B2iMzrE72qZUgzVvST2DzekUdCKypTYAXyT78XzD7RbXg6XGFIiQkzp2UJU-NQKaCK_TwvLARQzc0dgLVvaqp)



### #7 IAM 역할 생성

- Lambda 함수가 S3 버킷 및 Elastic Transcoder와 상호작용할 수 있도록 역할을 부여

  ![img](development-environment.assets/owsbDCI6IgFt4jP-sCjr6Nx142uE92rgzD-6Cdh8ZRNN8g8p3JryhrAv-i0-_YomWfP8hUcIdQ9iux1ShBPlfTkAuGx2LPSHCmv86LTD3XJ4QwKVE6pQWdAj94A9xCz_YPBIjnV8)

  ![img](development-environment.assets/YP8D3-mfFhJ_O7fz1T9TKbvj__Rt9-iJ1HDEH1JFqLSnSFFuyq3thzG29tGKFQIDETwG3ZcoHvHY7oY-CbFQx_7ooB_4XWh4qNUuZyzcLDyaFGpfWYZOAcS0fJhf5coPxBQVNSC3)

  ![img](development-environment.assets/CAdxoQIEL5SMtmuAc4tglb58czezu3j82KAZ-J8xOKgbM9_roDm9CbX8dHuzh1_UZ_HJZ4XmptU2sNDnIFIc-xKdC53vweXLLk2ztGmyZfbsWhre7B3dgWNHY1ppfFMSoJe8CVxd)

  - AWSLambdaExecute ⇒ Lambda 함수가 S3 및 CloudWatch와 상호작용 가능

  ![img](development-environment.assets/H3cvQoDdsVxoV4YYEjClLBGyi1mRGGzc2hXODpqBXssW0WyPb4RzCXY36zQORrwhT6YhQRVBP0tmoZMmP4fx-utu2idcW-5DR6yEFL_I8_iuNyZbCMaxy6rRydLNCe1snJdfTcQl)

  - AmazonElasticTranscoder_JobsSubmitter ⇒ Lambda 함수가 Elastic Transcoder에 새로운 트랜스코딩 작업을 전달 가능

  ![img](development-environment.assets/ZYR008FDh6X7faoKvWF3gkLzngCZHRAkxJarHrKhHgoJnWoOY8IVHpCN2Dj2wLnsiLQuBZkZItEshVHEiKm13ijFD26uYYGaMOdhQ_1oM2tIQY7PjzGWy4XLoeQ4rBvbz3B6-GN1)

  - Elastic Transcoder에 부여할 역할을 생성



### #8 람다 함수 생성 (p328)

> 업로드 버킷(serverless-video-upload-*)에 파일이 추가되면 Elastic Transcoder 작업을 시작하는 함수

![img](development-environment.assets/OCS0k8XvkQniCl707MDH3VjqqPB965JkLy4abfmIQH548euNdqoz7NpkdNcaAa6dtVpycKR6-HliUa5izyGbv7LMSeBK5Q1l_gAVSPUO0bQfI2BLqSXce0_qo-nOcdC0Ar7JRju_)

- 람다 함수 생성

  ![img](development-environment.assets/-gNZVALipvo1o8Mz3nMkQGWzk5J1WYBCQdm4HXJIX2CYxs_d5ahFgRPbzeyVUwy3UoymsAiwt39vEdpmMt-0oea19Bt_NxkldlcRn1oG0XQ2IcGRwO455yb1DO2XdF6a-C2uvhus)

  ![img](development-environment.assets/VNt5eCsi_gGhowvCT9hJ6OS4b6TqX6FpuH0trq2FqF0L9GEQOd6ByIK_wybRc07FEtqLwoxCrpDGV-i08iL2AiTcD1Z4OXCdF4_n2uWfmz45wP0rHeCVt-ORWKEyZV0kZ18vNLOF)

  ![img](development-environment.assets/CaBpdviUjZIpTHI8knsQaE5ptaqL2Llkv4_Cf2fR4cWx4s03qo6cJvY1EQGjxs8NeItThrTuVq0XLyM6-cqAc7OVHwZLgzERoa2EOt3v-QZerxkriMVo8DGA09j_5x73cF11tZu2)



### #9 Elastic Transcoder 구성 (p329)

> 다양한 형식과 비트 전송률로 비디오 트랜스코딩을 수행하기 위한 파이프 라인을 설정 ⇒ 업로드한 동영상의 포맷을 일관된 포맷으로 변경

- Elastic Transcoder 파이프라인 생성

  ![img](development-environment.assets/l6M4Sx__UCi3f_5ZmTEF1G6U9bXSAutYXXujiiCQ9UKebfMJh_qVzFSw4-wQutZigzYpweWJ7dgUNvfSucpOdj_5FxkEbpDhdS9cWJlKHxWrJXGFpPe1_f7EOORVxzGOyp46Tipf)

  ![img](development-environment.assets/QZjLIbjH26_CMa2t41OaKzhGsQa1oHOXGZL9aivPluXuVSCinJV8BdVFDfRZsWfTIfJrmWYc0_gQfPpUvi0TqWNPaGyz03YCOPVIaHgLV7MwuBpISXiuwha0Akqc6rmSimH8Kt_x)

  ![img](development-environment.assets/463VTrqLg6Rfmj2Gc-pgjaOGwzrctqwBCdwub4pyRpTbJuC0frON57ICzsCO0yVS0aaulpzqHn9qrjPl_p4wLr9BWP_TbewOxFt1my677DcyuBjBRxyMXqly7D1qbnFGdjV4bepm)

  ![img](development-environment.assets/o13jvJFw1tvkZiWFC_272Qs3aoedXPdKQyXUxHbYNPY1YL6AC_ktVLhgPvt60kSWgKSKWypgyh-tI51C-8wM6aBhaeZRBVAx90ONx47LPPqzznNPXd5gRhAbwZJW9Cu-2p1AIr_G)



### #10 npm 구성 (p330)

> npm(node package manager) 기반 AWS 환경을 위한 테스트, 패키징, 배포 과정을 자동화

- 패키지 설치

  - `cd \`

  - `mkdir serverless`

  - `cd serverless`

  - `mkdir transcode-video`

  - `cd transcode-video`

  - `npm init`

  - `type package.json`	

    ```json
    {                                                                   package.json 파일이 생성된 것을 확인
      "name": "transcode-video",
      "version": "1.0.0",
      "description": "",
      "main": "index.js",
      "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
      },
      "author": "",
      "license": "ISC"
    }
    ```

- node.js 기반의 AWS sdk 설치

  - `npm install aws-sdk`

  - `type package.json`

    ```json
    "dependencies": {
        "aws-sdk": "^2.861.0"        // AWS SDK 모듈 dependency 추가 확인
      }
    ```

    

### #11 zip 설치

> 윈도우에서 zip 명령어를 사용할 수 있도록 설치

- http://stahlworks.com/dev/zip.exe

- zip.exe 다운로드 후 `C:\Windows\System32` 디렉터리 아래로 복사

- Windows PowerShell을 새로 실행한 후 zip 명령어 실행 여부를 확인

  - `zip`

  