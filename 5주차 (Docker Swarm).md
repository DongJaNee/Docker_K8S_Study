# Docker Swarm 


<img width="892" height="515" alt="image" src="https://github.com/user-attachments/assets/76b3dc90-9244-408d-b928-bfa5c2e2331c" />

## MasterNode Set
```
docker swarm init --advertise-addr 192.168.0.107    # MasterNode IP주소

docker swarm join-token worker    # worknode등록을 위한 join token 정보 확인방법

docker swarm leave      # swarm 설정 종료 (추가적인 것)
```


<img width="919" height="91" alt="image" src="https://github.com/user-attachments/assets/f57dbf2d-03eb-4094-88ac-0f193d604881" />



### 위 토큰값을 worknode에 붙여넣기 
```
docker swarm join --token SWMTKN-1-255vhvfv4vclv8plae2zbfvknn1ehxyxnly7ba8q2slxl0t2b5-7r83sdrelf   6uooo3tjn1kkry8 192.168.0.107:2377
```

### MasterNode에서 노드 정보 확인 
```
docker node ls    # docker 연결 node 확인 
```


<img width="873" height="66" alt="image" src="https://github.com/user-attachments/assets/cbd96238-baa1-4621-8631-fc3401509ffb" />

---
## Docker Swarm web-server 컨테이너 실행 (MasterNode)
```
docker service create --name web-server -p 8080:80 reallinux/web-server:1    # nodejs http web-server 컨테이너 실행

docker service ps web-server     # service 컨테이너 확인

docker service rm web-server     # service 삭제 방법 (추가적인 것)
```

### Replicas 만들기 
```
docker service scale web-server=5     # 리플리카셋 생성

docker service ps web-server     # 복제본 확인
```


<img width="915" height="355" alt="image" src="https://github.com/user-attachments/assets/21298ceb-0ebc-44b2-a104-3e2b92c90c25" />


### 컨테이너 Rolling update
```
docker service update --image reallinux/web-server:2 web-server     # 버전1에서 -> 버전2로 업데이트

watch -n 1 docker service ps web-server     # Rolling update 확인
```

### 컨테이너 상태유지 기능 테스트
```
docker inspect [컨테이너 ID] | grep Pid     # 현재 실행중인 web-server 컨테이너 프로세스 ID 확인

sudo kill -9 [컨테이너 Pid]

watch -n 1 docker service ps web-server     # 상태 변화 확인 
```

<img width="614" height="551" alt="image" src="https://github.com/user-attachments/assets/8e1a5d53-9472-4fbf-aece-ece72995e4bb" />



