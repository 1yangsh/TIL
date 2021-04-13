## Cousul

> HashiCorp에서 개발했으며, 동적이고 분산된 인프라에서 애플리케이션을 연결하고 구성하기 위해 설계된 고가용성과 분산 환경을 지원하는 솔루션

### 주요기능

- Distributed Architecture
- Service Discovery
- Health Checking
- Key/Value Store
- Secure Communication

### 구성

- Consul Server
- Consul Agent

### Consul 설치

- http://www.consul.io/downloads.html

- Consul 기동
  - `consul agent -dev -ui -datacenter zone1 -node host1 -config-dir ./consul.d/`

- Consul Web-UI
  - http://localhost:8500/

### Consul commands

- `consul members`

  - consul 서버에 있는 멤버 확인

  

- set INV_SVC_URL=http://127.0.0.1:15001  

- set TAX_SVC_URL=http://127.0.0.1:15002



