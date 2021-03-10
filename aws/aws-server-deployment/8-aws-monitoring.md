## CloudWatch에 사용자 지정 지표 기록

### #1 사용자에게 CloudWatchEventsFullAccess 정책을 추가

- IAM 사용자

  ![img](8-aws-monitoring.assets/u8siF4bDUJe7OVVG8pDD5YQcZpmtCWnJFXXggTt1NPlZx6G7JGp4-YppYmB4agArPGZhuQcJq21pCM0Xm9OEAR29QV-u9iw6YKxo-JLRP6Kj1BgLmJNrif2R9a6JEuofYurgkeRV)

  **![img](8-aws-monitoring.assets/Hoe6yQRvcKT-k42Kxl7osal3m2JrKxr7qcEU0o__YlvBF4Gv8GnKltQN8NHkZvwiE0CQEI9-y209xQF0N_M33Yep885KQOTWQPZKy7z-iQUtCTQ36P5_MnIZa87ixJ4N9iIM6Bu_)**

  ![img](8-aws-monitoring.assets/9d9hwZMQynzv7xDzbV98rneIj6w-qv-L0ORYJbFiLqO4pActUenYTue5l_0YmyLSj83CVPljg9St0mhGr8-GNW5aEH5ubgxAriVD1TvpdiLQYrGXhpMSH4kpAbeJ1LzeTxykHrIj)



### #2 사용자 지정 지표를 생성해서 CloudWatch로 전송

- 인스턴스 시작

  ![img](8-aws-monitoring.assets/LyWdicAp3_5BRRKZNYoOahq6uL7AWrH7XrpgnyCQQw_VwbohBWUtcgOT5bc7R3mNos1Z5tTMWy25a2iOkK7pdOTUy-9IBDx3r_1ckE9y2dU-F6nUhvx2xfp3AIGQDfAOHtPqNtUL)

- SSH 접속

  ![img](8-aws-monitoring.assets/2BmbObpbBMUvZ3A2Q1QtsJ-eumHjsghRq1aTsxTYH3u2N_KfmChyxus2XFFH11VBGoAXiqj10T7j3imKydPZGqCgC8SSCm7h6WBEPE0wJiLZYLQ2bdaeD7RVz2Owjjj4PMfvQw3N)

  - 작업 디렉터리 생성 및 지표 데이터 파일 생성

    - $ `cd /var/www`

    - $ `mkdir cloudwatch-custom`

    - $ `cd cloudwatch-custom`

    - $ `vi test-data.json`

      ```json
      [
        {
          "MetricName": "People",
          "Dimensions": [{ "Name": "Gender", "Value": "All" }],
          "Timestamp": "2021-03-08T14:00:00.000+09:00",
          "Value": 20,
          "Unit": "Count"
        },
        {
          "MetricName": "People",
          "Dimensions": [{ "Name": "Gender", "Value": "All" }],
          "Timestamp": "2021-03-08T15:00:00.000+09:00",
          "Value": 24,
          "Unit": "Count"
        },
        {
          "MetricName": "People",
          "Dimensions": [{ "Name": "Gender", "Value": "All" }],
          "Timestamp": "2021-03-08T16:00:00.000+09:00",
          "Value": 30,
          "Unit": "Count"
        },
        {
          "MetricName": "People",
          "Dimensions": [{ "Name": "Gender", "Value": "All" }],
          "Timestamp": "2021-03-08T17:00:00.000+09:00",
          "Value": 23,
          "Unit": "Count"
        }
      ]
      ```

      - [참고: https://gist.github.com/deopard/76d334b9c4616c8e5e60429631c0f3b2]

        

- 지표 데이터를 CloudWatch로 전송

  - $ `aws configure`

    ```
    AWS Access Key ID [None]: AKIAS*******7DNCCN				
    AWS Secret Access Key [None]: UuW6AsjW1*****************nKqG1nr6pe2kR  
    Default region name [None]: us-east-1
    ```

  - $ `aws cloudwatch put-metric-data --namespace " Exercise People " --metric-data file://test_data.json`



### #3 CloudWatch에서 지표를 확인

- Cloudwatch 지표 확인

  ![img](8-aws-monitoring.assets/uC55jeRoTbSytOA6bbFs2x8P9C_ezqkvsqsJacw-F_JeGv-E4hF8lP3cPXlFKi8WC1O60JL2D8YQXCQDtAAyLMm4boG7rAFfg49-3Zl4rAKAKYsKdIO7G-3gTjWmxHYsw_M_bH21)

  ![img](8-aws-monitoring.assets/EIV0wsz0HevrYr3SFHZiE_JxFwkh4fYL0cZlu7XceqBlszUEwK-bwCu9vZanQ6axKA2cnZwDgMHjHCtDGgy5567k8U_Eb5v5hI560cW1vnlMRREnejbHV2SUjHQbg_AG3C085LTh)

  ![img](8-aws-monitoring.assets/jv1QB4SPfLhcnv84MU3LtLpIiW5q2jGbI_-Gfd3dG5aFSE3MBKOUZL0GTz0z2wZVU4_RqXOlXnSkNEJ2kOJcsObs_TYLpd7zn-ki5rSZJxiQ-5HAkz-KncSAKQl2JbgxF_tY1_Pw)



### #4 실습 환경 정리

- $ `cd /var/www`
- $ `rm -rf cloudwatch-custom/`

<br/>

---

<br/>

## CloudWatch Agent로 메모리, 디스크 사용량 지표, 로그 기록

> AWS 인프라 구축 가이드 (p199)

### #1 인스턴스에 IAM 역할 추가

- IAM 역할 만들기

  ![img](8-aws-monitoring.assets/erkQ54bySWfC_VUTcz9i90ZX-Hq0iN_Y9z0rAFLoLzNz8Bh7WqaIn-TxaJrsEbVGPrQcZyfHENrFlkgwmG6F0fKZmkJhsYwmuHItXqH5u3LFzlz72W-rB2YBzUPEYq1QOL6lOa3D)

  ![img](8-aws-monitoring.assets/rDpLjtivRK8aK9j1CtSfmk7vENBu9vaLe0XqrQA6SjXnOsZLPI_ZCCaBKM4n0iqYHPH5FOndRVegEBaBYoWLvK1PF8JLHXwJiERx88xSytJLcd91VGj9XR5SanDpkinJMJqabSCx)

  ![img](8-aws-monitoring.assets/8Tm9lKnmAvbwFUSZXerfH4KqeUhU6zOA7752xzfZ_dXPfUC8OogLDP1je4h6p4KdE8RZgsX82bkBRBIQ27zeBoZ24jtK8E9ipLE3zRhWVoaCRWQdu6JwcvaTle5bTkXAyncqM6Vz)

  

### #2 인스턴스의 IAM 역할을 변경(할당)

- IAM 역할 부여

  ![img](8-aws-monitoring.assets/SIK7ZhSWFCw2apVYhUSaXSDzLqmDV5ENrf6HqdBChKQyASu7nc007TffOpZ342zxZf4N59DgKLYzvbTgZHS0JSQafUMIwzOUFnO-avUgad_Tf-3hysHB3dOK15fU6b2-coqB8VJf)

  ![img](8-aws-monitoring.assets/r2uCDJ2xiLVppJvuLtg2cBr8PRySRQXin2HcwpOhCLHRRspCyt5ZAxMBBZmh600xIVgCDKIZ-ZT7TW1gKklfSra9E0Vr3YSpa3pYC4pfjUvX6igTJMlLNewwojKjFugCSUCyo-uU)

  ![img](8-aws-monitoring.assets/VNS51TNtfDIPSXUWpvUQin3Ntiy-sBDDY_FPmLVUBGVlEyVmFoln62s_xwVj22boEFVk--g4jcYD05Js8fccuCiRTDhWBwU7ET6pBKTClmCG1Q2k-Dvdhk8bQuKGIaMGzrLQlSJr)

  - 보안에서 IAM 확인

    ![img](8-aws-monitoring.assets/udz0dV8vJyaDuQ7BfrRgRS07pujra7yPgFDBmphY6c60AFRHSnlFuIPfwG34tbyUjy4w8EBjAW95HOZNXJS2rw0S-j2QSer8lM9wCeDUJpuxnU4yoyXvdZvOVgs-27Zv9ciYtd7w)



### #3 인스턴스에 CloudWatch 에이전트를 설치

> 참고 ⇒ https://docs.aws.amazon.com/ko_kr/AmazonCloudWatch/latest/monitoring/download-cloudwatch-agent-commandline.html

- 에이전트 설치
  - $ `sudo yum install amazon-cloudwatch-agent`
- 설치 마법사 실행 (p204)
  - $ `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-config-wizard`



### #4 설정 정보를 기반으로 에이전트를 실행

- 에이전트 실행

  - $ `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json -s`

- 실행 상태 확인

  - $ `sudo systemctl status amazon-cloudwatch-agent`

  - $ `sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status`

    

### #5 CloudWatch에 기록된 지표를 확인

- Cloudwatch에서 지표 확인

  ![image-20210310173627919](8-aws-monitoring.assets/image-20210310173627919.png)

  ![img](8-aws-monitoring.assets/zQrSZNL4gVOMxYrpRkvYHkGO9D6gNqog9x-YkqhWM-r0KTA5aOJgqfw1Dc0SG-v5Ckn-No0xHg32HhuUKTfH2c4jH52ff7d-TkY_Etd5S7RkSr_36M0RjtrbZ17OTwvTUFDfIRNu)

  ![img](8-aws-monitoring.assets/W_SYz_poMei72ltcMoPxB4ceclSzC2NW_f4eagjKRaWk8aC6asbrLb5eWHGOcquPMP_Vd55bJsIXvLYCHQX_pCxdEdbfS2_Ihm91XY9ZAg0VrzyaYFESBO4T8dJfJ_4o-vvYa8Nw)



### #6 실습이 끝나면 인스턴스 중지

- $ `sudo shutdown -h now`