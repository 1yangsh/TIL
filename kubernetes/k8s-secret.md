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

