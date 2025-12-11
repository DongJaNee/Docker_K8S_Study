## 쿠버네티스 기반 Rolling Update Test
```
# yaml 파일 수정
# reallinux/web-server:1 에서
# reallinux/web-server:2로 버전 업데이트

$ nano web-server-deploy.yaml

    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:2            //image: reallinux.web-server:1에서 2로 변경

$ kubectl apply -f web-server-deploy.yaml
$ watch -n -0.1 kubectl get pods,rs,deploy -o wide
```

<img width="942" height="489" alt="image" src="https://github.com/user-attachments/assets/ae405f3a-0964-46dc-809a-8bc1a694245c" />


<img width="944" height="683" alt="image" src="https://github.com/user-attachments/assets/acadd00c-cc12-4c0f-aa5d-d13026c1bccb" />
- 기존에 있던 web-server:1은 계속 실행되면서 web-server:2가 업데이트가 완료되면 바로 교체 될 수 있게 함
- 기존 버전을 서서히 새 버전으로 교체해 서비스 중단 없이 애플리케이션을 배포, 문제 발생 시 이전 버전으로 **빠르게 Rollback**하여 위험을 최소화하는 강력한 기능을 제공

## 쿠버네티스 기반 Rollback Test
```
# 롤백 진행
$ kubectl rollout status deploy/web-server-rs
$ kubectl rollout undo deploy/web-server-rs

# 버전 관리에 대한 히스토리 확인
$ kubectl rollout history deploy/web-server-rs

# Pod / ReplicaSet / Deployment 확인
$ watch -n 0.1 kubectl get pods,rs,deploy -o wide
```


<img width="1332" height="268" alt="image" src="https://github.com/user-attachments/assets/3c27237c-da50-4e9a-9309-8342047515c3" />


## 쿠버네티스 기반 Rollout 테스트
```
# 롤아웃 진행과 CNAME-CAUSE Annotation 기록 추가
$ kubectl set image deploy/web-server-rs web-server=reallinux/web-server:2 --record=true

# 버전 관리에 대한 히스토리 확인
$ kubectl rollout history deploy/web-server-rs

# Pod / ReplicaSet / Deploy 확인
$ watch -n 0.1 kubectl get pods,rs,deploy -o wide
```
<img width="786" height="88" alt="image" src="https://github.com/user-attachments/assets/a9837561-4582-4a7e-bcf2-fc3683b32338" />



<img width="1330" height="377" alt="image" src="https://github.com/user-attachments/assets/86d83314-94b8-4bd1-bee9-36a0560e08ef" />


<img width="1326" height="290" alt="image" src="https://github.com/user-attachments/assets/9696c17d-84b1-4b52-bbd4-bc090635be11" />


