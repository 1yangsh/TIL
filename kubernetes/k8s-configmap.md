## 컨피그맵(configmap)

> 포드에 설정값을 전달

- 컨피그맵 - 설정값
- 시크릿 - 노출되면 안되는 비밀값을 저장
- 네임스페이스 별도 존재

<br/>

#### 컨피그맵 생성

1. YAML 파일을 이용해서 생성
2. `kubectl create configmap` 명령어로 생성

- `kubectl create configmap log-level-configmap --from-literal LOG_LEVEL=DEBUG`
- `kubectl create configmap start-k8s --from-literal k8s=kubernetes --from-literal container=docker`
  - 키=값(key=value) 형식의 정보를 저장

#### 컨피그맵 확인

- `kubectl get configmap`
- `kubectl describe configmap log-level-configmap`
- `kubectl get configmap log-level-configmap -o yaml`

---

#### 포드에서 컨피그맵을 사용하는 방법

**포드에서 컨피그맵을 사용하는방법1. 컨피그맵의 값을 컨테이너의 환경변수로 사용**

1. YAML 파일을 작성

- `vi all-env-from-configmap.yaml`

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: container-env-example
  spec:
    containers:
      - name: my-container
        image: busybox
        args: ['tail', '-f', '/dev/null']
        envFrom:              # 컨피그맵에 정의되어 있는 모든 키=값 쌍을 가져와서 환경변수로 설정
        - configMapRef:
            name: log-level-configmap   # 컨피그맵 이름
        - configMapRef:
            name: start-k8s
  
  ```

2. 포드를 생성

- `kubectl apply -f all-env-from-configmap.yaml`

3. 컨테이너 환경변수를 확인

- `kubectl exec container-env-example -- env`

  ```
  # yaml 파일에 설정해놓았던 값을 확인
  LOG_LEVEL=DEBUG
  container=docker
  k8s=kubernetes 
  ```

- valueFrom, configMapKeyRef 

  - 컨피그맵에 존재하는 키=값 쌍중에서 원하는 데이터만 선택해서 환경변수로 설정

  1. YAML 파일을 작성

  - `vi selective-env-from-configmap.yaml`

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: container-env-example
  spec:
    containers:
      - name: my-container
        image: busybox
        args: ['tail', '-f', '/dev/null']
        env:
        - name: ENV_KEYNAME_1             # 새롭게 정의될 환경변수 이름
          valueFrom:
            configMapKeyRef:
              key: LOG_LEVEL              # 해당 컨피그맵에 정의된 변수의 이름(키)
              name: log-level-configmap   # 참조할 컨피그맵 이름
        - name: ENV_KEYNAME_2
          valueFrom:
            configMapKeyRef:
              key: k8s
              name: start-k8s
  ```

  2. 포드 생성 및 환경변수 확인

  - `kubectl apply -f selective-env-from-configmap.yaml`
  - `kubectl exec container-env-example -- env`

  ```
  ENV_KEYNAME_1=DEBUG
  ENV_KEYNAME_2=kubernetes
  ```

<br/>

**포드에서 컨피그맵을 사용하는 방법2. 컨피그맵의 값을 포드 내부의 파일로 마운트해 사용**

- volumeMounts

  - 모든 키-값 쌍 데이터를 포드에 마운트

  1. YAML 파일을 생성

  - `vi volume-mount-configmap.yaml`

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: configmap-volume-pod
  spec:
    containers:
      - name: my-container
        image: busybox
        args: ["tail", "-f", "/dev/null"]
        volumeMounts:
          - name: configmap-volume
            mountPath: /etc/config
    volumes:
      - name: configmap-volume
        configMap:
          name: start-k8s
  ```

  2. pod 생성

  - `kubectl apply -f volume-mount-configmap.yaml`

  3. pod 내부의  /etc/config 디렉토리를 조회

  - `kubectl exec configmap-volume-pod -- ls -al /etc/config`

  ```
  # 컨피그맵의 키 이름의 파일이 생성 되어진 것을 확인
  lrwxrwxrwx    1 root     root            16 Feb 24 05:47 container -> ..data/container
  lrwxrwxrwx    1 root     root            10 Feb 24 05:47 k8s -> ..data/k8s
  ```

  - `kubectl exec configmap-volume-pod -- cat /etc/config/container`

    ```
    docker
    ```

  - `kubectl exec configmap-volume-pod -- cat /etc/config/k8s`

    ```
    kubernetes
    ```

    - 파일 내용이 컨피그맵의 값과 동일

<br/>

#### 원하는 키-값 쌍의 데이터만 선택해서 포드에 마운트

1. YAML 생성

- `vi selective-volume-configmap.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-volume-pod
spec:
  containers:
    - name: my-container
      image: busybox
      args: ["tail", "-f", "/dev/null"]
      volumeMounts:
        - name: configmap-volume
          mountPath: /etc/config
  volumes:
    - name: configmap-volume
      configMap:
        name: start-k8s						# 컨피그맵의 이름
        items:				
        - key: k8s							# 컨피그맵에서 가져온 값의 키
          path: k8s-fullname				# 컨피그맵의 키에 해당하는 값을 저장할 파일 이름
```

2. pod 생성 후 마운트 디렉터리 및 파일을 확인

- `kubectl apply -f selective-volume-configmap.yaml`

- `kubectl exec configmap-volume-pod -- ls -al /etc/config`

- `kubectl exec configmap-volume-pod -- cat /etc/config/k8s_fullname`

  ```
  kubernetes
  ```

<br/>

#### 파일로부터 컨피그맵을 생성

>  nginx.conf (nginx 서버의 설정 파일) 또는 mysql.conf (MySQL 설정 파일) 등의 내용 전체를 컨피그맵에 저장할 때 사용

1. 설정 파일을 생성

- `echo Hello, World! >> index.html`

2. index.html 파일의 내용을 컨피그맵을 생성

- `kubectl create configmap index-file --from-file index.html`

3. 컨피그맵의 내용을 확인

- `kubectl describe configmap index-file`

```
Data
====
index.html:      // 파일 명이 컨피그맵의 키(key)로 사용
----
Hello, World!	 // 파일 내용이 컨피그맵의 값(value)로 사용
```

4. 키 이름을 직접 지정해서 컨피그맵 생성

- `kubectl create configmap index-file-customkey --from-file myindex=index.html`
- ` kubectl describe configmap index-file-customkey`

```
Data
====
myindex:			// 컨피그맵 생성시 사용한 키
----
Hello, World!
```

<br/>

#### 여러 개의 키-값 형식의 내용으로 구성된 설정 파일을 한꺼번에 컨피그맵으로 가져오기

1. 여러 개의 키-값 형식의 내용으로 구성된 설정 파일을 생성

- `vi multiple-keyvalue.env`

```env
mykey1=myvalue1
mykey2=myvalue2
mykey3=myvalue3
mykey4=myvalue4
```

- `kubectl create configmap from-env-1 --from-file multiple-keyvalue.env`
- `kubectl get configmap from-env-1 -o yaml`

```
data:
  multiple-keyvalue.env: |+		// 하나의 키
    mykey1=myvalue1				// 하나의 값	
    mykey2=myvalue2
    mykey3=myvalue3
    mykey4=myvalue4
```

---

- `kubectl describe configmap from-env-1`

```
Data
====
multiple-keyvalue.env:   // 하나의 키/값으로만 구성
----
mykey1=myvalue1
mykey2=myvalue2
mykey3=myvalue3
mykey4=myvalue4
```

- `kubectl create configmap from-env-2 --from-env-file multiple-keyvalue.env`
- `kubectl get configmap from-env-2 -o yaml`

```
data:
  mykey1: myvalue1	// 네개의 키/값으로 구성
  mykey2: myvalue2
  mykey3: myvalue3
  mykey4: myvalue4
```

<br/>

#### 컨피그맵을 생성하지 않고 YAML 파일로 생성

- `kubectl create configmap my-configmap --from-literal mykey=myvalue`
- `kubectl get configmap my-configmap`
- `kubectl get configmap my-configmap -o yaml > my-configmap.yaml`
- 마치 configmap 이 만들어진것 처럼(dry run) 하고 yaml 파일을 생성
  - ` kubectl create configmap my-configmap --from-literal mykey=myvalue --dry-run -o yaml`
- YAML형식의 출력을 파일로 저장 (리다이렉트)
  - `kubectl create configmap my-configmap --from-literal mykey=myvalue --dry-run -o yaml > my-configmap.yaml`
- YAML 파일을 이요해서 컨피그맵 생성
  - `kubectl apply -f my-configmap.yaml`
  - `kubectl get configmap`
  - `kubectl describe configmap my-configmap`

