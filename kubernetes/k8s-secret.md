## 시크릿 (Secret)

> 노출되면 안되는 비밀값을 저장

- SSH 키, 비밀번호 등과 같이 **민감한 정보**를 저장하는 용도로 사용

#### 타입

- Opaque 타입
  - 기본 타입
  - 사용자가 정의하는 데이터를 저장하는 일반적인 목적의 시크릿
- 비공개 레지스트리(private registry)에 접근할 때 사용하는 인증 설정 시크릿

<br/>

#### 시크릿 생성 방법

`--from-literal`

- 시크릿 생성
  - `kubectl create secret generic my-password --from-literal password=p@ssw0rd`
    - generic <=  Opaque 타입의 시크릿 생성을 명시
- 시크릿 확인
  - `kubectl get secrets`

**파일을 이용하여 패스워드 생성**

- 패스워드가 저장된 파일 2개 생성

  - `echo mypassword > pw1 && echo yourpassword > pw2`
  - `ls pw*`

- 시크릿 생성

  - `kubectl create secret generic our-password --from-file pw1 --from-file pw2`
  - `kubectl get secrets`

  ```
  our-password          Opaque                                2      5s
  ```

- 시크릿 내용 확인

  - `kubectl decribe secret my-password`

  ```
  # 컨피그맵과 다르게 내용이 노출되지 않는다
  Name:         my-password
  Namespace:    default
  Labels:       <none>
  Annotations:  <none>
  
  Type:  Opaque
  
  Data
  ```

  - `kubectl get secrets my-password -o yaml`

  ```
  apiVersion: v1
  data:
    password: cEBzc3cwcmQ=       <= BASE64로 인코딩
  kind: Secret
  metadata:
    creationTimestamp: "2021-02-24T07:05:58Z"
    name: my-password
    namespace: default
    resourceVersion: "262522"
    selfLink: /api/v1/namespaces/default/secrets/my-password
    uid: 16db2c87-b048-4c2d-8983-04e5362ed8c2
  type: Opaque
  ```

  <br/>

  - BASE64 인코딩 및 디코딩
    - `echo p@ssw0rd | base64`
      - => cEBzc3cwcmQK
    - `echo cEBzc3cwcmQ= | base64 -d`
      -  => p@ssw0rd

<br/>

### 시크릿에 저장된 값을 포드의 환경변수로 참조

**시크릿에 저장된 모든 값을 가져오기**

- `vi env-from-secret.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-env-example
spec:
  containers:
    - name: my-container
      image: busybox
      args: ["tail", "-f", "/dev/null"]
      envFrom:
        - secretRef:				# my-password 시크릿에 저장된 모든 키-값을 환경변수로 설정
            name: my-password
```

**시크릿에 저장된 일부 값을 가져오기**

- `vi selective-env-from-secret.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: selective-secret-env-example
spec:
  containers:
    - name: my-container
      image: busybox
      args: ["tail", "-f", "/dev/null"]
      env: 
        - name: YOUR_PASSWORD
          valueFrom:
            secretKeyRef:
              name: out-password
              key: pw2
```

<br/>

**시크릿에 저장된 값 전체를 포드에 볼륨 마운트**

- `vi volume-mount-secret.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-volume-example
spec:
  containers:
    - name: my-container
      image: busybox
      args: ["tail", "-f", "/dev/null"]
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret
  volumes:
    - name: secret-volume
      secret:
        secretName: our-password

```



**시크릿에 저장된 값 일부를 포드에 볼륨 마운트**

- `vi selective-volume-mount-secret.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: selective-secret-volume-example
spec:
  containers:
    - name: my-container
      image: busybox
      args: ["tail", "-f", "/dev/null"]
      volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret
  volumes:
    - name: secret-volume
      secret:
        secretName: our-password
        items:
        - key: pw1
          path: password1
```



**포드 생성 후 시크릿 확인**

- 포드 생성
  - `kubectl apply -f volume-mount-secret.yaml`
  - ` kubectl apply -f selective-volume-mount-secret.yaml` 

- 시크릿 확인

  - `kubectl exec selective-secret-volume-example -- ls -al /etc/secret`

    ```
    lrwxrwxrwx    1 root     root            16 Feb 24 08:06 password1 -> ..data/password1
    ```

  - `kubectl exec selective-secret-volume-example -- cat /etc/secret/password1`

    ```
    mypassword
    ```

    - 시크릿을 pod의 환경변수나 볼륨파일로 가져오면 BASE64로 디코딩된 값을 사용

<br/>

### 예제: nginx의 기본인증정보가 담긴 파일을 시크릿으로 관리

**접근통제 3단계**

- 식별(identification)
- 인증(authentication)
  - TYPE1 : 알고있는 정보 (지식기반) 
    - 패스워드
  - TYPE2 : 가지고 있는 정보 (소유기반)
    - 주민등록증, OTP, 인증서, 스마트폰
  - TYPE3 : 특징 (생물학적 특징을 이용 => 바이오 인증)
    - 홍채, 지문, 성문, 정맥,  … , 필기체 서명(싸인)
  - 2가지 이상을 혼용
    - 2-factor 인증
    - multi-factor 인증(=다중인증방식)
  - 멀티 디바이스 인증 = 멀티 채널 인증
- 인가(authorization)

<br/>

1. openssl 모듈을 이용해서 사용자명과 암호화한 패스워드를  BASE64로 인코딩

- `echo "your_name:$(openssl passwd -quiet -crypt your_password)" | base64`

2. 시크릿을 생성하는 YAML 파일을 작성

- `vi nginx-secret.yaml`

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: nginx-secret
type: Opaque
data:
  .htpasswd: eW91cl9uYW1lOmc4TnRBanYyY05iczIK
```

3. 시크릿 생성

- `kubectl apply -f nginx-secret.yaml`
- dry-run으로 YAML 파일 만들기
  - `kubectl create secret generic nginx-secret --from-literal .htpasswd=eW91cl9uYW1lOmc4TnRBanYyY05iczIK --dry-run -o yaml > test.yaml`
  - `cat test.yaml`

4. 시크릿을 활용해 기본인증을 적용하는 nginx 파드 구성

- `vi basic-auth.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: basic-auth
spec:
  type: NodePort
  selector: 
    app: basic-auth
  ports:
    - protocol: TCP
      port: 80
      targetPort: http
      nodePort: 30060
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: basic-auth
  labels:
    app: basic-auth
spec:
  replicas: 1
  selector:
    matchLabels:
      app: basic-auth
  template:
    metadata:
      labels:
        app: basic-auth
    spec:
      containers:
        - name: nginx
          image: "gihyodocker/nginx:latest"      
          imagePullPolicy: Always
          ports:
          - name: http
            containerPort: 80
          env:
          - name: BACKEND_HOST
            value: "localhost:8080"
          - name: BASIC_AUTH_FILE
            value: "/etc/nginx/secret/.htpasswd"  # (1) 기본 인증에 사용한 인증정보가 담긴 파일
          volumeMounts:
            - mountPath: /etc/nginx/secret    	  # 볼륨과 연결된 디렉터리 아래에 시크릿 키 이름의 파일이 생성 (1)
              name: nginx-secret                  # 볼륨 이름    
              readOnly: true
        - name: echo
          image: "gihyodocker/echo:latest"
          imagePullPolicy: Always
          ports: 
          - containerPort: 8080
          env:
          - name: HTTP_PORT
            value: "8080"
      volumes:
      - name: nginx-secret                        # 볼륨 이름
        secret:
          secretName: nginx-secret                # 시크릿 이름

```

5. 생성 및 확인

- `kubectl apply -f basic-auth.yaml`
- `kubectl get pods,deployments,services`

6. 인증 정보 없이 호출 시 401 오류 반환

- `curl -i http://127.0.0.1:30060`

7. 인증 정보와 함께 호출 시 정상 처리

- `curl -i --user your_name:your_password http://127.0.0.1:30060`
  - 인증 정보와 함께 전달 -> 200 상태 반환

