## kubernetes

> 컨테이너 운영을 자동화하기 위한 컨테이너 오케스트레이션 도구

<br/>

- 컨테이너 배포 및 배치 전략
- Scale in / Scale out
  - 트래픽에 따른 확장성
- Service discovery
  - MSA

- 명령행 도구 `kubectl`

---

- kubernetes 버전 확인

  - `kubectl version`

- `kubectl get nodes`

- `kubectl get pods`


<br/>

- 웹 서버 nginx 설치
  - `kubectl create deployment nginx --image=nginx`