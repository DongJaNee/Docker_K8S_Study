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
