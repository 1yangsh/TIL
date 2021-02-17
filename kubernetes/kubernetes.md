## kubernetes

> 컨테이너 운영을 자동화하기 위한 컨테이너 오케스트레이션 도구

<br/>

### Kubernetes란

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

---

### kubernetes 주요 개념

| Resource or Object      | 용도                                                         |
| ----------------------- | ------------------------------------------------------------ |
| Node                    | 컨테이너가 배치되는 서비                                     |
| Namespace               | 쿠버네티스 클러스터 안의 가상 클러스터                       |
| Pod                     | 컨테이너의 집합 중 가장 작은 단위, 컨테이너의 실행 방법 정의 |
| Replica Set             | 같은 스펙을 갖는 파드를 여러 개 생성하고 관리하는 역할       |
| Deployment              | 레플리카 세트의 리비전을 관리                                |
| Service                 | 파드의 집합에 접근하기 위한 경로를 정의                      |
| Ingress                 | 서비스를 쿠버네티스 클러스터 외부로 노출                     |
| ConfigMap               | 설정 정보를 정의하고 파드에 전달                             |
| Persistent Volume       | 파드가 사용할 스토리지의 크리 및 종류를 정의                 |
| Persistent Volume Claim | 퍼시스턴트 볼륨을 동적으로 확보                              |

