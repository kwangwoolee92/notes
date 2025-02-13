쿠버네티스 설치
==============

## 1. 메모리 swap 기능 비활성화 
### 1.1 swap 임시 비활성화
    - sudo swapoff -a

### 1.2 swap 영구 비활성화
    - sudo sed -i '/swap/s/^/#/' /etc/fstab

### 1.3 메모리 상태 확인
    - sudo free -m

### 1.4 swap 메모리 상태 확인, 출력값이 없으면 swap 메모리 비활성화 상태
    - sudo swapon -s
        
***
## 2. 방화벽 설정
### 2.1 방화벽 예외 설정(마스터, 워커)

    - sudo apt-get install -y firewalld
    - sudo systemctl start firewalld
    - sudo systemctl enable firewalld

    - sudo firewall-cmd --permanent --add-service=http
    - sudo firewall-cmd --permanent --add-service=https

### 2.2 마스터 노드일 경우
    - sudo firewall-cmd --permanent --add-port=6443/tcp
    - sudo firewall-cmd --permanent --add-port=2379-2380/tcp
    - sudo firewall-cmd --permanent --add-port=10250-10252/tcp
    - sudo firewall-cmd --permanent --add-port=8285/udp
    - sudo firewall-cmd --permanent --add-port=8472/udp
    - sudo firewall-cmd --reload

### 2.3 워커 노드일 경우
    - sudo firewall-cmd --permanent --add-port=10250/tcp
    - sudo firewall-cmd --permanent --add-port=30000-32767/tcp
    - sudo firewall-cmd --permanent --add-port=8285/udp
    - sudo firewall-cmd --permanent --add-port=8472/udp
    - sudo firewall-cmd --permanent --add-port=26443/tcp
    - sudo firewall-cmd --reload

### 2.4 포트 확인
    #열린 포트 확인
    - sudo firewall-cmd --list-all

    #열린 포트 확인
    - sudo netstat -tlnp

    #다른 노드의 포트 점검
    - telnet [ip] [port]
    - ex) telnet 192.168.100.128 6443

    >> 
    Trying 192.168.111.128...
    telnet: Unable to connect to remote host: Connection refused
    #Trying 192.168.111.128... 만 계속 나오면 방화벽 오픈되어 있지 않음
    #Connection refuesed가 나오면 방화벽 오픈은 되어 있으나 프로세스가 올라가 있지 않은 상태

    #방법2
    - curl -v telnet://[ip]:[port]

***
## 3.  네트워크 옵션 설정

### 3.1/etc/modules-load.d/k8s.conf 파일 생성
    - mkdir /etc/modules-load.d/k8s.conf

### 3.2 설정 세팅
    - sudo cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
      br_netfilter
      EOF

### 3.3/etc/sysctl.d/k8s.conf 파일 생성
    - mkdir /etc/sysctl.d/k8s.conf

### 3.4 설정 세팅
    - sudo cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
      net.bridge.bridge-nf-call-ip6tables = 1
      net.bridge.bridge-nf-call-iptables = 1
      EOF

### 3.5 시스템 재시작 없이 stysctl 파라미터 반영
    - sudo sysctl --system


***

## 4. containerD 설치

- ### apt 업데이트
  ```sudo apt-get update```

 - ### 필수 패키지 설치
    ```sudo apt-get install -y apt-transport-https ca-certificates curl gnupg```

 - ### 공개키 다운로드
    ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg```

 - ### 저장소 등록
    ```echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /devnull```

- ### 저장소 적용을 위한 apt 업데이트
    ```sudo apt-get update```

 - ### containerd 패키지 설치
    ```sudo apt-get install containerd```

- ### 설치 확인
    ```sudo systemctl status containerd```

- ### containerd 구성 파일 생성
    ```sudo mkdir -p /etc/containerd```

 - ### containerd 기본 설정값으로 config.toml 생성
    ```sudo containerd config default | sudo tee /etc/containerd/config.toml```

- ### config.toml 파일 수정
    ```vi /etc/containerd/config.toml```

- ### cgroup driver(runc) 사용하기 설정
    ```SystemdCgroup = true ```


 - ## 수정사항 적용 및 재실행
    ```sudo systemctl restart containerd```


## 5. 쿠버네티스 클러스터(Kubernetes Cluster) 설치
 
- ### apt 업데이트
    ```sudo apt-get update```

- ### 필수 패키지 설치
    ```sudo apt-get install -y apt-transport-https ca-certificates curl gnupg```
 
- ### 공개 키 다운로드
    ``` $ curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg```

- ### apt 저장소에 쿠버네티스 저장소 추가
    ```$ echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list```

- ### 저장소 추가되었기 때문에 apt 업데이트
    ```sudo apt-get update```

- ### 쿠버네티스 패키지 설치
    ```sudo apt-get install -y kubelet kubeadm kubectl```

- ### 쿠버네티스 패키지 버전 고정
    ```sudo apt-mark hold kubelet kubeadm kubectl```

- ### 쿠버네티스 설치 버전 조회
    ```
    kubelet --version
    kubeadm version
    kubectl version
    ```
- ### kubelet service 확인
    ```sudo systemctl status kubelet.service```

## 6. Master Node 구성

   - #### 쿠버네티스 클러스터를 초기화하여 새로운 마스터 노드를 생성하는 명령어
   - #### 별도의 옵션을 설정하지 않으면 기본 디폴트 값으로 적용
        ```    
        kubeadm init [옵션]

        #Flannel(디폴트 설정 값)
        sudo kubeadm init

        또는
        sudo kubeadm init --pod-network-cidr=10.244.0.0/16

        #Calico
        sudo kubeadm init --pod-network-cidr=192.168.0.0/16
        ```    
    
  - ### kubectl 설정
    ```
    sudo mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```
 - ### CNI(Container Network Interface) 설정 
   - Calico 설치
    ```
    curl https://docs.projectcalico.org/manifests/calico.yaml -O

    kubectl apply -f calico.yaml
    ```
    - Flannel 설치
    ```
    kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
    ```

## 7. Node 구성
 - ### Worker Node 구성
   - ### Master Node 구성시 마스터와 연결할  정보(ip, port, 토큰)가 나오는데 이걸 워커 노드 구성시 사용용
    ```
    sudo kubeadm join [ip]:6443 --token [토큰명] --discovery-token-ca-cert-hash [hashkey]
    
    kubeadm join 192.168.0.34:6443 --token qwxpe1.mq1onyinohcd42ns \
            --discovery-token-ca-cert-hash sha256:544278f7d0c45c2f037dd17cf06c95125dbf425c5be39c9bd0175e24aaadb782
            
    ex) kubeadm join 192.168.0.34:6443 --token 9atfnk.gymsmdlh6rscndvy \
            --discovery-token-ca-cert-hash sha256:6c55364fbc8561b3acdf1f49cbd818afda2868f79372f3a8048a7bd1368c7eb4
    ```		
 - ### 쿠버네티스에서 마스터 노드가 싱글 노드, 때 테인트제거 방법

    ```kubectl taint nodes --all node-role.kubernetes.io/control-plane-```

## 8. 오류 날 시 해결법
 - ### 8.1 서버 재부팅시 -kubernetes-api-server 가연결이 안되면 메모리 swap 문제 일수 있음 
   - k8s는 swap 이 있는경우 정상 동작 안함 
   - ### 1.1 과 1.2 를 진행 해줘야함
  
 - ### 8.2 인증서 오류 날때 임시 방법
   ```
   kubectl config set-cluster kubernetes --insecure-skip-tls-verify=true
   
   kubectl config set-cluster kubernetes--certificate-authority=/path/to/ca.crt --server=https://192.168.0.34:6443
   ```