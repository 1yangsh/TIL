## 서비스 (Service)

> 포드를 연결하고 외부에 노출

- YAML 파일에 containerPort 항목을 정의했다고 해서 해당 포트가 바로 외부에 노출되는 것은 아님
- 해당 포트로 사용자가 접근하거나, 다른 디플로이먼트의 포드들이 내부적으로 접근하려면 서비스(service) 객체가 필요

<br/>

### 서비스의 기능

- 여러 개의 포드에 쉽게 접근할 수 있도록 고유한 도메인 이름을 부여
- 여러 개의 포드에 접근할 때, 요청을 분산하는 로드 밸런서 기능을 수행
- 클라우드 플랫폼의 로드 벨런서, 클러스터 노드의 포트 등을 통해 포드를 외부에 노출

### 서비스의 종류(type)

- ClusterIP 타입
  - 쿠버네티스 내부에서만 포드들에 접근할 때 사용
  - 외부로 포드를 노출하지 않기 때문에 쿠버네티스 클러스터 내부에서만 사용되는 포드에 적합
- NodePort 타입
  - 포드에 접근할 수 있는 포트를 클러스터의 모든 노드에 동일하게 개방
  - 외부에서 포드에 접근할 수 있는 서비스 타입
  - 접근할 수 있는 포트는 램덤으로 정해지지만, 특정 포트로 접근하도록 설정할 수 있음
- LoadBalancer 타입
  - 클라우드 플랫폼에서 제공하는 로드 밸런서를 동적으로 프로비저닝해 포드에 연결
  - NodePort 타입과 마찬가지로 외부에서 포드에 접근할 수 있는 서비스 타입
  - 일반적으로 AWS, GCP 과 같은 클라우드 플랫폼 환경에서 사용

<br/>

### 디플로이먼트를 생성

- `vi deployment-hostname.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hostname-deployment
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
          image: alicek106/rr-test:echo-hostname  # 파드의 호스트 이름을 반환하는 웹 서버 이미지
          ports:
            - containerPort: 80
```

- 디플로이먼트 실행
  - `kubectl apply -f deployment-hostname.yaml`

<br/>

### 클러스터 노드 중 하나에 접속해서 curl을 이용해 포드에 접근

- `kubectl run -i --tty --rm debug --image=alicek106/ubuntu:curl --restart=Never curl 192.168.104.36 | grep Hello`

**1. CluserIP 타입의 서비스 - 쿠버네티스 클러스터 내부에서만 포드에 접근**

- hostname-svc-clusterip.yaml 파일을 생성

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hostname-svc-clusterip
spec:
  ports:
  - name: web-port
    port: 8080              # 서비스의 IP에 접근할 때 사용할 포트
    targetPort: 80			# selector 항목에서 정의한 라벨의 포트의 내부에서 사용하고 있는 포트
  selector:					# 접근 허용할 포드의 라벨을 정의
    app: webserver
  type: ClusterIP			# 서비스 타입
```

**2. 서비스 생성 및 확인**

- `kubectl apply -f hostname-svc-clusterip.yaml`
- 서비스 확인
  - `kubectl get services`
  - = `kubectl get svc`

**3. 임시 포드를 생성해서 서비스로 요청**

- `kubectl run -it --rm debug --image=alicek106/ubuntu:curl --restart=Never -- bash`
- `ip a`
- `curl 10.109.166.250:8080 --silent | grep Hello`
  - 명령어를 칠때마다 각각 다른 노드에게 할당
    - 서비스와 연결된 포드에 `load balancing`을 수행

**4. 서비스 이름으로 접근**

- `curl hostname-svc-clusterip:8080 --silent | grep Hello`

  *쿠버네티스는 어플리케이션이 서비스나 포드를 쉽게 찾을 수 있도록 내부 DNS를 구동*

  - 따라서 이름으로도 ip로 접근하는 것과 동일한 결과 값을 냄

**5. 서비스 삭제**

- `kubectl delete service hostname-svc-clusterip`

