## 네임스페이스 (namespace)

> 쿠버네티스 클러스터 안의 가상 클러스터

- 포드, 레플리카셋, 디플로이먼트, 서비스 등과 같은 쿠버네티스 리소스들이 묶여 있는 하나의 가상 공간 또는 그룹 리소스를 논리적으로 구하기 위한 오브젝트

#### 네임스페이스 조회

- `kubectl get namespaces`

#### 특정 네임스페이스에 생성된 포드를 확인

- `kubectl get pods`
- = `kubectl get pods --namespace default`
  - 네임스페이스를 지정하지 않으면 기본적으로 default 네임스페이스를 사용
- kube-system 네임스페이스 확인
  - `kubectl get pods --namespace kube-system`
    - 쿠버네티스 클러스터 구성에 필수적인 컴포넌트와 설정 값이 존재하는 네임스페이스
    - `--namespace` == `-n`
- 네임스페이스의 서비스를 확인
  - `kubectl get services -n kube-systme`

---

#### 네임스페이스 생성 및 확인

1. YAML 파일 작성

- `vi production-namespace.yaml`

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: production
```

2. 네임스페이스 생성 및 확인

- `kubectl apply -f production-namespace.yaml`
- `kubectl create namespace mynamespace`
- `kubectl get namespaces`

```
mynamespace       Active   9s
production        Active   39s
```

3. 특정 네임스페이스에 리소스를 생성하는 방법

- `vi hostname-deploy-svc-ns.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hostname-deployment-ns
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      name: my-webserver
      labels:
        app: webserver
    spec:
      containers:
      - name: my-webserver
        image: alicek106/rr-test:echo-hostname
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: hostname-svc-clusterip-ns
  namespace: production
spec:
  ports:
    - name: web-port
      port: 8080
      targetPort: 80
  selector:
    app: webserver
  type: ClusterIP
```

- `kubectl apply -f hostname-deploy-svc-ns.yaml`
- `kubectl get pods,svc --namespace production`

4. 모든 네임스페이스의 리소스를 확인

- `kubectl get pods --all-namespaces`

5. 동일 네임스페이스 내의 포드에 접근

- `kubectl get services --namespace production`

```
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
hostname-svc-clusterip-ns   ClusterIP   10.110.139.176   <none>        8080/TCP   3m29s
```

- `kubectl run -it --rm debug --image=alicek106/ubuntu:curl --restart=Never --namespace=production -- bash`
- `curl 10.110.139.176:8080`
  - = `curl hostname-svc-clusterip-ns:8080`

6. 다른 네임스페이스 내의 서비스에 접근

- `kubectl run -it --rm debug --image=alicek106/ubuntu:curl --restart=Never -- bash`
  - 네임스페이스를 지정하지 않고(=default 네임스페이스) 포드를 생성
- `curl hostname-svc-clusterip-ns:8080` #  <= X 서비스 이름으로 접근 불가능
- `curl 10.110.139.176:8080`
- 다른 네임스페이스에 존재하는 서비스로 접근하기 위해서는 `[서비스이름].[네임스페이스이름].svc` 형태로 사용
  - `curl hostname-svc-clusterip-ns.production.svc:8080`

---

#### 네임스페이스 삭제

1. 네임스페이스 이름으로 삭제

- `kubectl delete namespace mynamespace`

2. 만들기 위한 YAML 파일을 통한 삭제

- `kubectl delete -f production-namespace.yaml`

#### 네임스페이스에 속하는 오브젝트 종류

- `kubectl api-resource --namespace=true`

#### 네임스페이스에 속하지 않는 오브젝트 종류

- `kubectl api-resources --namespaced=false`
  - 클러스터 전반에 사용되는 경우가 많음