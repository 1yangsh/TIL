## Deployment

> 레플리카셋, 포드의 배포, 업데이트 등을 관리



### 디플로이먼트 생성, 삭제

- `vi deployment-nginx.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx
    spec:
      containers:
        - name: nginx
          image: nginx:1.10
          ports:
          - containerPort: 80

```

- `kubectl apply -f deployment-nginx.yaml`

- 디플로이먼트 삭제

  - `kubectl delete deployment my-nginx-deployment`
    - 디플로이먼트 삭제 -> 레플리카셋, 포드도 함께 삭제됨

- 포드의 이미지를 변경 

  - `kubectl set image deployment my-nginx-deployment nginx=nginx:1.11 --record`
    - nginx 이미지를 1.10->1.11로 변경

- 리비전 히스토리 확인

  - `kubectl rollout history deployment my-nginx-deployment`

  ```
  1         kubectl apply --filename=deployment-nginx.yaml --record=true
  2         kubectl set image deployment my-nginx-deployment nginx=nginx:1.11 --record=true
  
  ```

- 특정 리비전으로 롤백

  - `kubectl rollout undo deployment my-nginx-deployment --to-revision=1`

- 이전 리비전으로 롤백

  - `kubectl rollout undo deployment my-nginx-deployment`

