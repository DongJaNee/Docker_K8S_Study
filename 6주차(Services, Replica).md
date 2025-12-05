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

### Pod 의 컨테이ㅏ너 내부로 접속 후 확인하기
```
kubectl exec -it web-server -- bash
ps -ef | grep node
```

<img width="643" height="93" alt="image" src="https://github.com/user-attachments/assets/74374b29-0199-45f9-b2eb-df7d0e86ca01" />



