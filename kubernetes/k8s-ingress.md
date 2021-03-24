## Ingress

> HTTP 경로의 라우팅 규칙을 정의

- Ingresss-Controller
  - 규칙들을 동작
- Ingress-nginx
  - Ingress와 Ingress Controller 연동



- Ingress-controller 적용
  - $ `kubectl apply -f  https://raw.githubusercontent.com/kubernetes/ingressnginx/nginx-0.27.1/deploy/static/mandatory.yaml`



- Ingress-nginx 적용
  - $ `kubectl apply -f  https://raw.githubusercontent.com/kubernetes/ingressnginx/nginx-0.27.1/deploy/static/provider/baremetal/servicenodeport.yaml`



- Ingress 확인
  - $ `kubectl get ingress --all-namespaces`
- Ingress Pod 확인
  - $ `kubectl get pod --namespace ingress-nginx`
- Service 확인
  - $ `kubectl get service --namespace ingress-nginx`