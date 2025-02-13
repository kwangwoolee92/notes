kubectl 유용한 명령어
===
### 1. 약어및 자동 완성
```
# 약어
sudo vi ~/.bashrc
alias k='kubectl'
source ~/.bashrc (적용)

# 자동 완성
sudo apt update
sudo apt install -y bash-completion


echo 'source <(kubectl completion bash)' >> ~/.bashrc
echo 'alias k=kubectl' >> ~/.bashrc
echo 'complete -o default -F __start_kubectl k' >> ~/.bashrc
source ~/.bashrc
``` 
---
### 2.  pod안에 들어가는 방법
    kubectl exec -it <pod-name> -- /bin/bash
---
### 3. 로그 확인 명렁어
    # 파드 <pod-name>에서 로그의 스냅샷을 반환한다.
    kubectl logs <pod-name>

    # 파드 <pod-name>에서 로그 스트리밍을 시작한다. 이것은 리눅스 명령 'tail -f'와 비슷하다.
    kubectl logs -f <pod-name>

    # 파드의 컨테이너에 대한 로그를 출력한다.
    kubectl logs <pod-name> -c <pod-container>

### 4. secret 에서 base64로 인코딩 되어 있는 텍스트트 디코드
    kubectl get secret coup-backend-secret -n coup -o jsonpath='{.data.ENVIRONMENT}' | base64 --decode

    