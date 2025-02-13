ingress controller 설치 방법
===
## 1. nginx ingress controller 설치 방법
### 1.1 nginx Ingress Controller 설치를 위한 YAML 파일을 다운로드
    wget https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.0/deploy/static/provider/cloud/deploy.yaml
 
### 1.2 YAML 파일을 사용하여 Nginx Ingress Controller를 배포
    kubectl apply -f deploy.yaml
 
### 1.3 설치를 확인
    kubectl get pods -n ingress-nginx