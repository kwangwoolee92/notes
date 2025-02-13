postgresql 설치
==============

## 1. helm 사용하여 설치
### 1.1 Bitnami 레포지토리 추가
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm repo update

### 1.2 PostgreSQL 설치
    helm install postgres bitnami/postgresql

    helm install <pod 이름> bitnami/postgresql --namespace <namespace 이름> --create-namespace

### 1.3 설치 후 확인
    kubectl get pod
    
### 1.4 pvc 및 pv 확인
    설치후 pvc  확인
    kubectl get pvc

    pvc yaml 확인
    kubectl get pvc <pvc-name> -o yaml

    pvc 오류 확인하고 pv 를 생성하여 공간 확보
    - pv.yaml
    
    apiVersion: v1
    kind: PersistentVolume
    metadata:
    name: postgres-pv
    spec:
    capacity:
        storage: 8Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: "/mnt/data"

    - pv 적용 
    kubectl apply -f ./pv.yaml
        
### 1.5 서비스 nodeport 변경 방법        
    kubectl patch svc <svc 이름> -n <namespace 이름> -p "{\"spec\": {\"type\": \"NodePort\"}}"

### 1.6 pod 진입 방법
    kubectl exec -it <확인한_파드_이름> -- /bin/bash

### 1.7 DB 연결 
    psql --host 127.0.0.1 -U postgres -d postgres -p 5432