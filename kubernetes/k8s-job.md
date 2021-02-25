## 잡(job)

> 하나 이상의 파드를 생성해 지정된 수의 파드가 정상 종료될 때까지 이를 관리하는 리소스

- 잡이 생성한 파드는 정상 종료 후에도 삭제되지 않고 남아 있어 로그나 실행 결과를 분석할 수 있다.
- 배치 작업 형태에 적합

- `vi simple-job.yaml`

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: pingpong
  labels:
    app: pingpong
spec:
  parallelism: 3						# 동시에 실행하는 포드의 수 (=병렬로 실행)
  template:								# 포드를 정의
    metadata:
      labels:
        app: pingpong
    spec:
      containers:
        - name: pingpong
          image: gihyodocker/alpine:bash
          command: ["/bin/bash"]  
          args: 
          - "-c"
          - |
            echo [`date`] ping!
            sleep 10
            echo [`date`] pong!
      restartPolicy: Never				# 포드 종료 후 재실행 여부 설정 (Always, Never, OnFailure)
      									# 포드는 Always가 기본값
      									# 잡은 Always로 설정 불가, Never, OnFailure만 설정 가능
```

- 파드 생성

  - `kubectl apply -f simple-job.yaml`

- 로그 확인

  - `kubectl logs -l app=pinpong`

  ```
  [Wed Feb 24 10:35:37 UTC 2021] ping!	← (1) 37초
  [Wed Feb 24 10:35:47 UTC 2021] pong!
  [Wed Feb 24 10:35:39 UTC 2021] ping!	← (2) 39초
  [Wed Feb 24 10:35:49 UTC 2021] pong!
  [Wed Feb 24 10:35:53 UTC 2021] ping!	← (3) 53초
  [Wed Feb 24 10:36:03 UTC 2021] pong!
  ```

- 상태 확인

  - `kubectl get pods -l app=pingpong -o wide`

<br/>

### 크론잡(CronJob)

> 정기적으로 포드를 실행

- 잡은 한 번만 실행되는 반면, 크론잡은 스케줄을 지정해 정기적으로 포드를 실행
- cron 등을 사용해 정기적으로 실행하는 작업에 적합

- YAML 파일 생성

  ```
  # 크론탭 날짜 지정 방식
  *　　　　　　*　　　　　　*　　　　　　*　　　　　　*
  분(0-59)　　시간(0-23)　　일(1-31)　　월(1-12)　　　요일(0-7)
  ```

  - `vi simple-cronjob.yaml`

  ```yaml
  apiVersion: batch/v1beta1
  kind: CronJob
  metadata:
    name: pingpong
  spec:
    schedule: "*/1 * * * *"                    # 파드를 실행할 스케줄을 정의
    jobTemplate:                               # 파드를 정의
      spec:
        template:
          metadata:
            labels:
              app: pingpong
          spec:
            containers:
              - name: pingpong
                image: gihyodocker/alpine:bash
                command: ["/bin/bash"]
                args: 
                - "-c"
                - |
                  echo [`date`] ping!
                  sleep 10 
                  echo [`date`] pong!
            restartPolicy: OnFailure
  ```

  - 실행

    - `kubectl apply -f simple-cronjob.yaml`

  - 확인

    - `kubectl get job -l app=pingpong`

    ```
    pingpong-1614219360   1/1           21s        2m6s         <= 약 60초 간격으로 실행
    pingpong-1614219420   1/1           15s        66s
    pingpong-1614219480   0/1           5s         5s
    ```

  - 로그 확인

    - `kubectl logs -l app=pingpong`

    ```
    [Thu Feb 25 02:22:09 UTC 2021] ping!
    [Thu Feb 25 02:22:19 UTC 2021] pong!
    [Thu Feb 25 02:23:10 UTC 2021] ping!
    [Thu Feb 25 02:23:20 UTC 2021] pong!
    [Thu Feb 25 02:24:12 UTC 2021] ping!
    [Thu Feb 25 02:24:22 UTC 2021] pong!
    [Thu Feb 25 02:25:15 UTC 2021] ping!
    ```

    

