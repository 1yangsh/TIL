## Monitoring

> AWS 리소스의 로그 관리, 이벤트 모니터링 서비스

###  CloudTrail

- AWS 리소스의 모든 읽기, 쓰기 작업의 상세 로그(작업 내역, 관련 리소스와 리전, 작업 수행자의 작업 시간 등)를 보관
- API  작업과 비 API 작업을 모드 기록
  - API 작업 
    - 예 : 인스턴스 시작, S3 버킷 생성, VPC 생성
  - 비 API 작업
    - 예 : AWS Management Console에 로그인 등
- 이벤트(event)
  - AWS 계정의 활동 기록
  - `관리 이벤트` = 제어 플레인 작업(Control Plane Operations)
    - 보안 주체가 AWS 리소스에서 실행하는 작업을 포함
      - 보안 구성 
        - → IAM AttachRolePolicy API
      - 디바이스 등록 
        - → EC2 AttachRolePolicy API
      - 데이터 라우팅 규칙 구성  
        - → EC2 CreateSubnet API
      - 로깅 설정  
        - → CloudTrail CreateTrail API  
    - 계정에서 발생하는 비 API 이벤트
      - 사용자가 로그인하는 경우 ConsoleLogin 이벤트가 로깅
    - 쓰기 전용과 읽기 전용으로 분류
      - 쓰기 전용 이벤트 : 리소스를 변경하거나 변경할 수 있는 API 작업
      - 읽기 전용 이벤트 : 리소스를 읽기만하고 변경하지 않는 API 작업
  - `데이터 이벤트` = 데이터 플레인 작업(Data Plane Operations)
    - 리소스 또는 리소스 내에서 수행되는 리소스 작업에 대한 정보를 제공
    - 대량의 작업이 수행되는 S3 객체 수준 활동, Lambda 함수 실행
      - S3 객체 수준 활동 → GetObject, DeleteObject, PutObject API
      - Lambda 함수 실행  →  Invoke API
    -  추적을 생성할 때 데이터 이벤트는 기본적으로 기록되지 않음
      - 데이터 이벤트를 기록하려면 활동을 수집할 리소스 또는 리소스 유형을 추적에 명시적으로 추가해야 함
  - `인사이트 이벤트`
    - AWS 계정의 비정상적인 활동을 캡쳐
    - 계정 API 사용량 변화가 계정의 일반적인 사용 패턴과 크게 다를 때 로깅
      - S3 deleteBucket API 호출이 평균적으로 분당 20회 호출 
        - → 분당 100회 호출이 감지 
        - ⇒ 비정상적인 활동
        - ⇒ 비정상적인 활동이 시작될 때와 정상으로 돌아갔을 때를 기록

   

- 이벤트 기록 (Event History)
  - CloudTrail 이벤트에 대한 지난 90일 간의 기록
  - 조회, 검색, 다운로드 등이 가능
  - 각 리전별로 __이벤트 기록__을 작성하고 해당 리전에서의 활동만 기록
  - IAM, Route 53 등의 글로벌 서비스 이벤트는 모든 리전의 이벤트 기록에 포함

- 추적 (trail)
  - 90일이 경과한 이벤트 기록을 저장하거나 CloudTrail이 기록하는 이벤트 유형을 사용자 정의할 때 생성
  - 특정 이벤트를 기록하고 지정한 S3 버킷에 CloudTrail 로그 파일을 전달, 로그 파일에는 JSON 형식의 항목이 하나 이상 들어 있음
    - eventTime
    - userIdentity
    - eventSource
    - eventName
    - awsResion
    - sourceIPAddress  

  

### CloudWatch

- AWS 리소스와 AWS에서 실시간으로 실행되는 애플리케이션을 모니터링
- 리소스와 애플리케이션에 대한 지표(= 측정할 수 있는 변수)를 수집하고 추적
- CloudWatch 웹 사이트에는 사용 중인 모든 AWS 서비스에 한 지표가 자동으로 표시되고, 사용자 지정 대시보드 추가가 가능
- 지표를 감시해 알림을 보내거나 임계값을 위반한 경우 모니터링 중인 리소스를 자동으로 변경하는 경보를 생성
- 시스템 전체의 리소스 사용량, 애플리케이션 성능 및 운영 상태를 파악

<br/>

---

<br/>

## LAB1 : Monitoring and Notifications with CloudWatch Events and SNS

- LAB 링크
  - https://learn.acloud.guru/course/178db59b-70f1-4bd8-8d74-9ab9263f8f9a/learn/6bc3035e-205b-4a57-bcfe-b5ded5593bcb/35c57628-7e64-41f6-8d37-59017a9cbeb4/lab/9087f514-28eb-4ace-acd4-b6cb83f666a0

- EC2 인스턴스가 중지(shutdown)되었을 때 이메일 통지(notification)를 발생

![img](aws-monitoring.assets/D_5vjoN5ZQUE-d2wfI0zSNUEvHr568GW-lABN-s_4XM3hOKYe8Doa9W-ASKmqU4Hxx9p1za6ZNwbqqQxhgzEHuRl5ZPvav-NOJVCb8TKNMdkWZUWX8kC87i2yVXsqA9nR2rTrdA7)

#### #1 EC2 인스턴스 확인

![img](aws-monitoring.assets/SmiyUJ_pQ6Qi5p7mynm__qdk6rVHgDRKW-jLzdK_iy5xnokVK27DFJEG3zq69nOju8NkpgEUptA_FCzEaPsXV7E0yjigqncQk6xE8K2AJc72v6uVO6v_l-xuRRC7U1dwojw3JrJU)



#### #2 SNS 주제(topic)와 이메일 구독(subscribe)을 생성

![img](aws-monitoring.assets/bTfnhNP3vVPxJ8NdhdSfDYe6M2zjTc854bgsOCv_oCvb66kTFYaH_DyXwzRyTZYIamkc6lq2JA0a3vxy7f6FgebZjGAEHGkqRrMTCUJkIZASSMJKIqwfAWxuDFSQKR5Gu3XzwEGh)

![image-20210305101223966](aws-monitoring.assets/image-20210305101223966.png)

![img](aws-monitoring.assets/gCXuPerVdSLq2cS379YdPZbUiNtxAYFJr_xzQAQ7FJR4tuAtKpbMyKrdh5YZ8EOTtYjMwDPMZpsEKb8wtd1VCnqjBU-o-6lfMOlgoYi7ZZUOqfhbO9tMTXTvYvFUCpMKKx6-i9bJ)

  

#### #3 SNS 주제를 트리거하는 CloudWatch 이벤트 규칙을 생성

![img](aws-monitoring.assets/DYN5b75TLgPLaJ2-jLEDA9ccPCBMjQ1fx-fS0VDGWadpWFgD1Am5c6V9lrA9wB6MhgjHA9uuhchoVoTtPRVJC8I8qLogmOmPKtE_G8AwKYD_Hb7x8u5a9gaW1YT8ueJpZUPFuiYF)

  

#### #4 EC2 인스턴스의 상태를 변경했을 때 이메일 통지가 수신되는지 확인

- 인스턴스 중지

![img](aws-monitoring.assets/L-CA_w36-Ws4WE2XIJlFnqlt3Z9CDn7QY8fFvPFidHAP8cVK9xurA8wdMfnk_HsSfYvbCxDnxz0hudoo73k8ltdlewD7sT_PjY7t5AyxSpdTWX1ridkJt9ljYGYn5tpyw6FjdRd0)

![img](aws-monitoring.assets/iuKvacZjkbqhy1fuaQTH9lI9VyXx7kccqah8HLq2N1QEI6YvUtrrx9qqCWmgOBcR-D7m7xxQOnJVq8jPT0x7dYwOx4pMYtQPpjwZXRM16zA5RZ2IdLT2Ibv0y8zh-GqOVUim4O6j)

- 인스턴스 실행

![img](aws-monitoring.assets/tslp8Jo1rIG_X8yYULFRvyPe5ogx7EILtm_mgfN1prDo7n-9WyrtQdpFBjrZaZ5WbkSkqDdRMDIunk97poLi11bXLKwfN8tSC2HCGNgtzDvosz00jfegFFOGZHP-MYi-Rl7-yaT5)

![img](aws-monitoring.assets/RmcXbodiRvWKaj2AWjXzMOtXhxrYLQYMnpcZk2Q6cq4-SnhFzvpGK2V-bEDuyIJcBinMxq9IGas_fNMlR_JM7CPaWkSmp-n11_ytg8GB2rge-LUZAam_IPeZvLxHEjoZ0sZbhM0H)

<br/>

---

<br/>

## LAB1-2 : EC2 인스턴스가 중지(stopped)되었을 때 이메일과 함께 SMS 통지를 발생하도록 설정

- SMS 구독 생성

![img](aws-monitoring.assets/8JT13hdNqTKY4WnW1UnJBg5Bbynl9kcR8t0FeS-eXsmOvTwSU8U97-NcfUhUOnwtOkXU-IW6q3GYKlvh-9gTI51isQEf2HAvZyjJZwnNhHNbgcZV-WhI_2G-6mHUH7paFGJO_6KH)

- 이벤트 수정

  - 이벤트 유형 -> 특정상태(shutdown)

  - 일치하는 이벤트의 일부 -> JSON에서 detail만 받아 오기 위해

    ```json
    "detail":{"instance-id":"i-0697bdc417cb5d414","state":"stopping"}}
    ```

![img](aws-monitoring.assets/Lu_GReLGITe8R_AplWyT-xpOadmM1iCCcPOErYCqyOGHg51k-22Vnwr1u-dPk2SRaW0exUCcK0Rwp4p45f56vkA3YDEOnoV32LRcxs3MXbvD0Al0o795XdAaDglMdvXZEowCqtIH)

- 이메일 확인

![image-20210305104419363](aws-monitoring.assets/image-20210305104419363.png)



- 문자 확인

![image-20210305104204790](aws-monitoring.assets/image-20210305104204790.png)

#### TODO. EC2 인스턴스가 중지(stopped)되었을 때 포맷팅된 알림 메시지를 전송하시오.

<br/>

---

<br/>

## LAB2 : AWS EC2 Custom Logging with CloudWatch

> 인스턴스에서 생성되는 로그 정보를 CloudWatcch로 전송해서 로그를 통합

- EC2 인스턴스에 CloudWatch Logs 에이전트를 설치하고, 로그 서비스를 켜고, 메시지를 수신하도록 CloudWatcch를 구성

#### #1 EC2 인스턴스 생성

![img](aws-monitoring.assets/4BZ7dovRou1eKYS9Tt0DECg4r3qTaYcmAZ4ymrTQSQzmSRgV10zB_DU9QHgOPKmu-uv0fDaaSq26ZWwlSnS28UAqPjc7N22tpCM-GfAmqoVX1PLv0NGUNGcPAZcnkT6g9beuR6PH)

**![img](aws-monitoring.assets/uDlBkp9PqBI1tYtzU81kGpb5PF0Majw-oI0J0AkvFXbIR2qFJEujVdSGI7MdgLRSFJsXeCNyav1RkUXhyXF1QIhvo11lS_AV6U33ypO4-MLy7r6Cvp3U1Z-QIOfwE-M5T-8IfRrT)**

---

![img](aws-monitoring.assets/Xu7joDqdlNjeGdwan4gbuQjkHZEHLvRR5jWgYDhGaldteFtJKXUbPUc57DWJCbaq93ahD9wHq4oPGiVN4ff2W0Pgmwx1Ne1qWDDkaPIv11xAeeRzPpl0HDlc7aRLFcs2pNAWOcjH)

  

#### #2 작업을 위해 EC2 인스턴스에 SSH 접속

![img](aws-monitoring.assets/DvFW-zA7FQv6C3id2fu4J7YxK1_g1AFhvncO2rX1GcjZ_TOc3PToxjwQ84CkL6Pr7aUGsaXI9meCjDqzWLBHVKXPwyO5V2iWkfXuzoO0sFGhV1Acxi5owRSEteeVOZS1ee_C2tBf)

![img](aws-monitoring.assets/5cj4VsmiDCoRcpa86mWtFhqVunWbYFSw7kQP8DuH1hxKziONZc1yJ8IxkpFtXPejkB37NqJaSP_RXAw3ISF0wApQJcsseayNHcO9hi4clWog2jmVYXXYyiDkQsEGxHJBYr29kSbS)

  

#### #3 EC2 인스턴스에 awslogs 서비스를 추가

- [ec2-user@ip-10-0-0-134 ~]$ `sudo yum update -y`

- [ec2-user@ip-10-0-0-134 ~]$ `sudo yum install -y awslogs`

- [ec2-user@ip-10-0-0-134 ~]$ `cd /etc/awslogs`

- [ec2-user@ip-10-0-0-134 awslogs]$ `ls -l`

  ```
  total 20
  -rw------- 1 root root   55 Mar  5 02:14 awscli.conf	⇐ 자격증명과 지역정보를 포함
  -rw-r--r-- 1 root root 8355 Jul 25  2018 awslogs.conf	⇐ CloudWatch 로깅에 대한 설정 정보를 포함
  drwxr-xr-x 2 root root    6 Jul 25  2018 config
  -rw-r--r-- 1 root root  147 Jul 25  2018 proxy.conf
  ```

- [ec2-user@ip-10-0-0-134 awslogs]$ `sudo systemctl start awslogsd	`

  - awslogs 서비스를 시작

- [ec2-user@ip-10-0-0-134 awslogs]$ `tail -f /var/log/awslogs.log`

  - awslogs 에이전트가 생성하는 로그를 확인

  ```
  2021-03-05 02:18:18,404 - cwlogs.push - INFO - 3448 - MainThread - Missing or invalid value for use_gzip_http_content_encoding config. Defaulting to use gzip encoding.
  2021-03-05 02:18:18,404 - cwlogs.push - INFO - 3448 - MainThread - Missing or invalid value for queue_size config. Defaulting to use 10
  2021-03-05 02:18:18,404 - cwlogs.push - INFO - 3448 - MainThread - Using default logging configuration.
  2021-03-05 02:18:18,425 - cwlogs.push.stream - INFO - 3448 - Thread-1 - Starting publisher for [1538ea66cfd1d4424de78dedc63516f8, /var/log/messages]
  2021-03-05 02:18:18,431 - cwlogs.push.stream - INFO - 3448 - Thread-1 - Starting reader for [1538ea66cfd1d4424de78dedc63516f8, /var/log/messages]
  2021-03-05 02:18:18,432 - cwlogs.push.reader - INFO - 3448 - Thread-4 - Start reading file from 0.
  2021-03-05 02:18:24,519 - cwlogs.push.publisher - WARNING - 3448 - Thread-3 - Caught exception: An error occurred (ResourceNotFoundException) when calling the PutLogEvents operation: The specified log group does not exist.
  2021-03-05 02:18:24,520 - cwlogs.push.batch - INFO - 3448 - Thread-3 - Creating log group /var/log/messages.
  2021-03-05 02:18:24,582 - cwlogs.push.batch - INFO - 3448 - Thread-3 - Creating log stream i-04545e7d208a38d21.
  2021-03-05 02:18:24,675 - cwlogs.push.publisher - INFO - 3448 - Thread-3 - Log group: /var/log/messages, log stream: i-04545e7d208a38d21, queue size: 0, Publish batch: {'skipped_events_count': 0, 'first_event': {'timestamp': 1614910316000, 'start_position': 0L, 'end_position': 162L}, 'fallback_events_count': 0, 'last_event': {'timestamp': 1614910698000, 'start_position': 68477L, 'end_position': 68564L}, 'source_id': '1538ea66cfd1d4424de78dedc63516f8', 'num_of_events': 789, 'batch_size_in_bytes': 88289}
  ```

- [ec2-user@ip-10-0-0-134 awslogs]$ `sudo systemctl enable awslogsd.service`
  - 부팅 시 awslogs 서비스를 자동 실행

  

#### #4 EC2에서 보낸 CloudWatch Logs 확인 (수집된 로그를 확인)

- EC2 인스턴스의 /var/log/message 파일의 내용과 CloudWatch의 로그 그룹에 수집된 내용이 일치

  ![img](aws-monitoring.assets/q7gxE8GbWFWRfBmiOZREiIX2qr6fdYxhcAyKY9JEae73SCk9tUE-45FVZUO7YFb007IRvlEq4EsCXJCa85NUsuFtY26G1Oc1iVcv6GnLewtTjS6vGJq0af1aBEUI1P8rahXOM7Sn)

  ![img](aws-monitoring.assets/kodyfgCZS47qgIHfmGv4CWwcrHGdWKzJSHLh0hDzWgJxjajmjFH98un9KIBEczncrFzvX5i9zQGf5uw6n2Fw6DyqOnhZbfkbSIr0Af7HSsG4ESJ5neJuI0HtHetNXQO-b72aNvSI)

- 참고 :
  
  - https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/QuickStartEC2Instance.html

<br/>

---

<br/>

## LAB3 : AWS Access Control Alerts with CloudWatch and CloudTrail

- https://learn.acloud.guru/course/178db59b-70f1-4bd8-8d74-9ab9263f8f9a/learn/6bc3035e-205b-4a57-bcfe-b5ded5593bcb/d604c083-853e-453d-8f21-bc65e2c70aee/lab/a3839dd5-7088-4941-9e7e-fd04f006ccd2

![image-20210305133839606](aws-monitoring.assets/image-20210305133839606.png)

#### #1 중요 정보를 담을 S3 버킷 생성 ← 모니터링이 필요

![img](aws-monitoring.assets/c1aTYc7RZjQ-ZikFfEx_tPq3llckg2-anzxXRAniXxbNqWibE6TpnsXOaWWD8zOO_4E_rHf9OywPZU_jdKEwKlMVJdDxL7yqq_g3vHw_zLrqmngZUejz1BTGyJ8NfjT8zTiA0PrJ)



#### #2 CloudTrail 서비스에 추적(trail)을 생성

![img](aws-monitoring.assets/NLcO4OOduQcrlpbjrod3Grsuw4FCCfb0zLiWyBBLfiZvGuZVfhqM6cgx1EemwX4NdBKQC4Dyc38SIDgDSe0GNnCdlw7TfevzCa1T3JkJ0q7BK6cUaY-ZPj9FnikJCINqreAuvC-C)

![img](aws-monitoring.assets/Z80p6Ck67vIWw4RquX7I2og5WcNjjyDTpBGF8cYITFnFDJn-retRsEKH0P6X6_vdmR8IwcWdu0irkOy4fhNC3eY5SKa42R0yT7m0MD0BWFdVMtflvskkK0anpfsrwQy_jVOGBdrb)

![img](aws-monitoring.assets/4a9qNQQp8TbbRttnLH5YzWWmFxeBHTu5WdzWIEw8s6TNR5TQBgPy1cSW6E7Y36fQioi2_CPvK_Su_kESXKPVu3RP1NNKkhdiGP_VsxpEmD24UjNr9FGzJOSy3LHpuuwffN3rRgkd)

#### 

### #3 CloudWatch 로그 그룹을 생성

![img](aws-monitoring.assets/TSRd-6oANy1Aiw7EG5Ar7in4h63Q6My6qDHL9EQjBwTtfH0wzRUcq2OKDeZ1iXV8r-s_HTQ9eNsxGRenV3X2JClYeEqbGrGm7cG1MBJAqfBnKo67ve7OC6v8XF1wnOBb2UAffZvz)

![img](aws-monitoring.assets/7zwES8y3Frn7OpN17dBzg3r3XTmyuJJPxjwPwyou_HZqKsQ3-HLPoPuS7fjKOq-QkitVTaQIcAV2z3wuq14pgAhV_LKZts2c1lOU49VldjvhV4k0DaycV7RXiMI-VqBBwN4r2xfb)

  

#### #4 CloudWatch에서 지표를 설정

![img](aws-monitoring.assets/SwvOQfDaeJhRaSEhMnKwyTJGKmfjp3LoFYfbj57IFQ4SDQz0LjXslWk0SILIK04lXD64IBtc4y3hcFahZrlOIr_IUXxFkjLgH3vyiu0Uzb71RSvBeMcwtxBtaOz4Zuxr5fLG7zUF)

- 패턴 필터링

```
{ ($.eventSource = s3.amazonaws.com) && (($.eventName = PutObject) || ($.eventName = GetObject)) }
```

![img](aws-monitoring.assets/Mj2y9veVOgjyZzz35sABd6zWZpDyICK6a3fg8H14X-M563qG2c8c4YLRG84eqcAihe2ZpsdXlzoIUQf0T-4J7ffViS-A5yPSlFlLFoiKWyKV1G35LFZVwFbzNALGS0ryEsB0T44I)

![img](aws-monitoring.assets/FIcA__9c4_yKzHlqv3JzGLAC4gScFbHJeF1A2ZkPzOF0Ff5WuiXXeQTPYUF30NLgreexli51bU9FXZsDFubNUTGXnMVA7VM1kjKMQVr4paAnrltU2-b8VlOOr9AYRjL6hhx2-GRw)

  

#### #5 CloudWatch에서 경보를 설정

![img](aws-monitoring.assets/tDU0i6_g1_LLJP7YcEOhnhHrR4HDgOXwxUol_6EqIyIq7BQ19lJZ2gsXjOCuo5ZO3Q6QQEoBBbUBmDV3z3M1Nt4gvep2llr7NGY4zLknNzoeUlGkvk1x3GP5mAvsdcjKssYzN7_8)

![img](aws-monitoring.assets/GXGYYktHoyqqzUXEZv6KscvzhbiP1wDrDJ5xVk4tGSGrv58viRQxXaF-LBzkNeUWmT4GO7v8lp5fHtmTjbzFaunBK6eZMHxibbuwG4UhlboQJlcN4FTIy13Q3vqdGVN0ift1EUHM)

![img](aws-monitoring.assets/yA8iAdl0_POrnt-T9fwfiQSMY-8Y2vEcv3ch94lqnluI8SUrHV7zjzCiCeztRmPPPDw-XZ5rjoYiV-_wfYbP_EcVGoq1C_lDbSJlvkQGO3ZMdSE5eejp8GcqYay9PR-0bCXU3Dfm)

---

- 경보를 알릴 데이터 포인트가 3/3으로 설정된 경우

![img](aws-monitoring.assets/8P2WEJT5XHSqiqIIS5cp0Fbp6WnTXeulO2mtoZ7eU3rVo3pbwmV9qCXyF8NC2ttf3C_NYb15-OmMBtvSCTS4HY1VHaExgqZ9SDozc0TK5BQmyy5EwiCzJ3qHzyz5zyNGBQomu_Qr)

- 지표 경보 상태
  - OK									
    - 지표 또는 표현식이 정의된 임계값 내에 있음
  - ALARM 					      
    - 지표 또는 표현식이 정의된 임계값을 벗어났음
  - INSUFFICIENT_DATA
    - 경보가 방금 시작되었거나, 지표를 사용할 수 없거나, 지표를 통해 경보 상태를 결정하는데 충분한 데이터가 없는 경우

![img](aws-monitoring.assets/45bIvrqVZjCgXLzePO4EfrrCLgJhXXfy7TEeWovHqmU3Xm8LWHTc-93IiKItihjrPG7SUX9dLuCgYMgNy5LFWvr3aXFszeumxmfBXjupx_Q8JLxbx1iyXseHlWhVvm2-2ssUyccW)

  

#### #6 S3 버킷에 파일을 업로드 후 경보 발생 여부 확인

> 파일 업로드 시, 설정한 경보를 확인한다

![img](aws-monitoring.assets/xjRRAJQCC8IVxR_-EyWwYYtdtZvn7i_8nNAZGYekQNVuZ2eEujgqFpVyHMkxdiqv8d1n9JRnIMfse5tOCD3cfG0Q-NdcFjulB3z0E-n5eSTTmW4FicX7QJN6D483l8gqJ6CNp1XM)

![img](aws-monitoring.assets/mzDvwbgdDPJjDVcsmubFkYWpafzLQYwM1cnF0ohzDZ-Bt5AH3BAm3DTafh_a9zYr-tK0kzjlSyENivJJzmCJTBl_aFW6zJWBJAcOX0Ci0vl-NZbrpEqZUFlZEEl0BAMJGw4OZ4XW)