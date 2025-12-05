## K3s 설치
```
curl -sfL https://get.k3s.io | sh -    //k3s install

sudo systemctl status k3s    //설치된 k3s 확인

sudo sh /usr/local/bin/k3s-uninstall.sh    //※참고 : k3s 삭제
```

<img width="647" height="206" alt="image" src="https://github.com/user-attachments/assets/4b5786f5-9287-4a96-a033-da0ceb3a255a" />

### 노드 확인 
```
kubectl get node
sudo chmod +r /etc/rancher..k3s/k3s.yaml
kubectl get node

kubectl get pod -A     //쿠버네티스의 모든 Pod 확인
```

<img width="532" height="59" alt="image" src="https://github.com/user-attachments/assets/c713976f-b5b8-4fda-b34b-92bae076af99" />



<img width="641" height="284" alt="image" src="https://github.com/user-attachments/assets/e0568a79-eb3c-4399-bb88-0f44bd2f6161" />


## Work 노드 추가하기 
```
sudo cat /var/lib/rancher/k3s/server/node-token     // Masternode의 토큰 내용 복사 (Masternode에서 명령어 실행)

curl -sfL https://get.k3s.io > install_k3s.sh      // Worknode에서 k3s 설치 스크립트 받기 (Worknode에서 명령어 실행)

K3S_URL=https://"마스터 노드 IP 주소":6443 K3S_TOKEN="Master노드토큰" sh install_k3s.sh    //Masternode의 IP 주소와 토큰을 Worknode 에서 k3s-agent 설치 (Worknode에서 명령어 실행)


sudo sh /usr/local/bin/k3s-agent-uninstall.sh     //※참고 : k3s-agent 삭제
```

<img width="640" height="78" alt="image" src="https://github.com/user-attachments/assets/d837f822-8d95-4f07-8769-69c1e88b1f68" />

### Label 만들기
```
kubectl get node
kubectl label node sub-node node-role.kubernetes.io/worker=worker      //sub-node를 worker로 label 붙이기
kubectl get node
```

### Label, Node 삭제
```
kubectl label node sub-node node-role.kubernetes.io/worker-        // label 삭제 명령어

kubectl delete node sub-node       //node 삭제 명령어 
```

## 쿠버네티스 기반 웹서버 컨테이너 실행 (hello-world)
```
# hub.docker.com에 업로드 되어있는 hello-world 이미지 Pod로 실행하기

kubectl run hello-world --image hello-world:nanoserver-ltsc2025

kubectl get pods
```

<img width="1893" height="605" alt="image" src="https://github.com/user-attachments/assets/939ee7f8-5830-4e93-acae-559c67490561" />



<img width="801" height="136" alt="image" src="https://github.com/user-attachments/assets/2ca3e2d1-efb9-426a-93f5-6ba57f358586" />

### 현재 Pod 확인정보 자세히 보기 
```
kubectl get pods -o wide
kubectl describe pods hello-world 
```

## 쿠버네티스 기반 웹서버 컨테이너 실행 (web-server)
```
kubectl run web-server --image reallinux/web-server:1

kubectl get pods
```

<img width="642" height="127" alt="image" src="https://github.com/user-attachments/assets/e69425e9-bd32-4194-a9f6-16b7dc4344d2" />

### Pod 의 컨테이너 내부로 접속 후 확인하기
```
kubectl exec -it web-server -- bash
ps -ef | grep node
```

<img width="643" height="93" alt="image" src="https://github.com/user-attachments/assets/74374b29-0199-45f9-b2eb-df7d0e86ca01" />


## Pod를 yaml 파일을 통해서 실행하기
```
kubectl delete pods web-server       //현재 실행중인 Pod 삭제하기

nano web-server-pod.yaml     //yaml 파일 생성하기

apiVersion: v1
kind: Pod
metadata:
  name: web-server
  labels:
    app: web
spec:
  containers:
    - name: web-server
      image: reallinux/web-server:1
      ports:
        - containerPort: 80
          protocol: TCP
```

<img width="640" height="384" alt="image" src="https://github.com/user-attachments/assets/67a4e78e-023a-4c7f-b31d-cf943b0bd78b" />

```
kubectl apply -f web-server-pod.yaml     // yaml 파일을 통해서 Pod 실행

kubectl delete -f web-server-pod.yaml     // yaml 파일을 통해서 pod 삭제

kubectl get pods       //pod 확인
```

<img width="644" height="78" alt="image" src="https://github.com/user-attachments/assets/de8229b2-1066-41a4-a96b-0b841006969d" />

## Pod와 Service 세팅
```
kubectl delete -f web-server-pod.yaml       //현재 실행중인 Pod 삭제

nano web-server-pod.svc.yaml      //yaml 파일 생성

apiVersion: v1
kind: Pod
metadata:
  name: web-server
  labels:
    app: webapp
spec:
  containers:
    - name: web-server
      image: reallinux/web-server:1
      ports:
        - containerPort: 80
          protocol: TCP
---
apiVersion: v1
kind: Service          
metadata:
  name: web-server
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 80
      protocol: TCP
      nodePort: 31000
  selector:
    app: webapp
```

```
kubectl apply -f web-server-pod-svc.yaml      //yaml 파일을 통해서 Pod 실행

kubectl delete -f web-server-pod-svc.yaml       //yaml 파일을 통해서 Pod 삭제

kubectl get pods,service      //Pod와 Service 확인 
```

<img width="641" height="240" alt="image" src="https://github.com/user-attachments/assets/884756c1-8abc-4b7b-aecb-0dd1fa37a647" />


### Pod와 Service 결과: 웹서버 접속 테스트 
```
kubectl describe pod web-server | grep IP | head -1        //Pod의 IP 주소로 직접 웹서버 요청 테스트

curl 10.42.0.17      

curl localhost:31000        //호스트에서 직접 웹서버 요청 테스트 (Node Port)

kubectl get service        //Service의 Cluster IP 주소 알아내기 

curl 10.43.64.37:8080      //Service의 Cluster IP 주소로 웹서버 요청 테스트 
```

<img width="644" height="206" alt="image" src="https://github.com/user-attachments/assets/e98fe934-1ffe-48b2-8a68-9b6bee193a51" />


## Pod 복제 Replkica 5개로 유지하기
```
kubectl delete -f web-server-pod-svc.yaml      //현재 실행중인 Pod 삭제

nano web-server-replicaset.yaml       //replicaset yaml파일 생성


apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-server-rs
spec:
  replicas: 5

  selector:
    matchLabels:
      app: webapp
      tier: app
  template:
    metadata:
      labels:
        app: webapp
        tier: app
    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:1
          ports:
            - containerPort: 80
              protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: web-server
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 80
      protocol: TCP
      nodePort: 31000
  selector:
      app: webapp
      tier: app
<img width="387" height="1067" alt="image" src="https://github.com/user-attachments/assets/1229e797-7075-429c-b0ef-4450e705a2d6" />
```

```
kubectl apply -f web-server-replicaset.yaml       //yaml파일을 통해서 pod 실행

kubectl delete -f web-server-replicaset.yaml       //yaml파일을 통해서 pod 삭제

kubectl get pods,rs        //pod와 replicaset 확인
```

<img width="639" height="232" alt="image" src="https://github.com/user-attachments/assets/556e9f02-7ce4-4982-b04c-5ac46b76d8e0" />

### master노드와 work노드에 있는 Pod 확인 
```
kubectl describe node | grep -e "Pods:\|Name:\|Roles:"      // 각 노드(master, worker)에 있는 pod 확인하기

kubectl describe node | grep -A 7 "Pods"       //실행중인 기본 Pod와 web-server Pod 확인
```

<img width="643" height="145" alt="image" src="https://github.com/user-attachments/assets/7f8e5934-9207-4e8a-9806-023ea586e4d8" />


<img width="744" height="473" alt="image" src="https://github.com/user-attachments/assets/4eb82673-1752-4dfe-af5c-92749b1b3a32" />


### 서비스 강제종료 테스트 및 상태 유지 확인 
```
watch -n 0.1 kubectl get pods,rs -o wide        //master노드 기준으로 현재 Pod 확인

ps -ef | grep node       //work노드 기준으로 Pod 프로세스 강제종료 실험

sudo kill -9 <PID>
```

<img width="939" height="365" alt="image" src="https://github.com/user-attachments/assets/cdf60bbe-76f2-4f4c-9741-6ece3739b135" />


<img width="654" height="412" alt="image" src="https://github.com/user-attachments/assets/11beedad-5453-4c9c-a82f-92ac8ee48ef8" />


<img width="938" height="348" alt="image" src="https://github.com/user-attachments/assets/12c40a18-892f-4046-94f4-f576f677a0a0" />
- ERROR가 떳다가 다시 살아나는 것을 볼 수 있다.
