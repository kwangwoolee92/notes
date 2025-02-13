harbor 설치
==============
## 1. Harbor Helm 차트 추가
    helm repo add harbor https://helm.goharbor.io

## 2. Harbor 네임스페이스 생성
    kubectl create namespace harbor

## 3. Harbor 설치
    helm install harbor harbor/harbor -n harbor --set expose.type=ingress --set expose.tls.enabled=false

## 4. Harbor 설정
  - Harbor의 설정을 위해 harbor-data-values.yaml 파일을 생성하고 필요한 설정을 추가합니다.
  - 중요 설정: harborAdminPassword, secretKey, database.password, core.secret, core.xsrfKey, jobservice.secret, registry.secret
hostname 설정: hostname: harbor.yourdomain.com
  - TLS 설정 : 
자체 서명 인증서를 사용하는 경우, 인증서를 생성하고 tls.crt, tls.key, ca.crt 설정에 추가합니다.
  - Harbor 배포
 
        helm upgrade --install harbor harbor/harbor -f harbor-data-values.yaml -n harbor