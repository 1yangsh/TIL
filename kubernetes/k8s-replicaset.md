## 레플리카셋(Replica Set)

> 정해진 수의 동일한 포드가 항상 실행되도록 관리

- 노드 장애 등의 이유로 포드를 사용할 수 없다면 다른 노드에서 포드를 다시 생성

<br/>

### 레플리카셋 생성 및 삭제

- `vi replicaset-nginx.yaml`

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: replicaset-nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx-pods-label
  template:                         ⇐ 포드 스펙, 포드 템플릿 → 생성할 포드를 명시
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx-pods-label
    spec:
      containers:
        - name: my-nginx-container
          image: nginx:latest
          ports:
          - containerPort: 80
            protocol: TCP
```

-  레플리카셋 생성

  - `kubectl apply -f replicaset-nginx.yaml`

- 레플리카셋 확인

  - `kubectl get  pods, replicaset`

- 포드 개수를 증가시킨 후 실행

  - `cp replicaset-nginx.yaml replicaset-nginx-4pods.yaml`
  - `vi replicaset-nginx-4pods.yaml`

  ```yaml
  apiVersion: apps/v1
  kind: ReplicaSet
  metadata:
    name: replicaset-nginx
  spec:
    replicas: 4
    selector:
         :
  
  ```

  - `kubectl apply -f replicaset-nginx-4pods.yaml`

  ```
  replicaset.apps/replicaset-nginx configured		⇐ 기존 리소스를 수정 (포드 개수를 3개에서 4개로 조정)
  ```

- 레플리카셋 삭제
  - `kubectl delete rs replicaset-nginx`

<br/>

### 레플리카셋 동작 원리

- app: my-nginx-pods-label 라벨을 가지는 포드를 생성

  - `vi nginx-pod-without-rs.yaml`

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: my-nginx-pod
    labels:
      app: my-nginx-pods-label
  spec:
    containers:
      - name: my-nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
  ```

- `kubectl apply -f nginx-pod-without-rs.yaml`

- 라벨을 함께 출력

  - `kubectl get pods --show-labels`

  ```
  NAME           READY   STATUS    RESTARTS   AGE     LABELS
  hello-pod      1/1     Running   2          3d21h   app=hello
  my-nginx-pod   1/1     Running   0          14s     app=my-nginx-pods-label   <===
  nginx-test     1/1     Running   4          4d4h    run=nginx-test
  pod-1          2/2     Running   6          3d22h   <none>
  ```

  

