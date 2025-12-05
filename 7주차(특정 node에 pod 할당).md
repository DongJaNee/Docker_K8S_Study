<img width="1097" height="234" alt="image" src="https://github.com/user-attachments/assets/1226d8eb-ee91-47cc-bb2f-5b478a950f4c" />## Master노드와 Worker노드에 각각 라벨 설정 
```
kubectl label node <name> type=cpu      //서버가 CPU 특화 되어있다는 의미의 라벨 (마스터 노드 기준으로 라벨 설정)
kubectl describe node <name> | less

kubectl label node <name> type=gpu      // 서버가 GPU 특화 되어있다는 의미의 라벨 (마스터 노드 기준으로 라벨 설정)
kubectl describe node <name> | less 

kubectl delete -f web-server-replicaset.yaml         //기존에 생성했던 replicaset 삭제

nano web-server-deploy.yaml        //yaml파일 생성하기

apiVersion: apps/v1
kind: Deployment
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
```

```
kubectl apply -f web-server-deploy.yaml        //yaml 파일을 통해서 pod 실행하기

kubectl delete -f web-server-deploy.yaml         //yaml파일을 통해서 pod 삭제

kubectl get pods,rs         //pod와 replicaset 확인
```

### CPU중심적인 서버 type=cpu에 pod 생성하기
```
nano web-server-deploy.yaml

    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:1
          ports:
            - containerPort: 80
              protocol: TCP
      nodeSelector: 
        type: cpu

kubectl apply -f web-server-deploy.yaml
watch -n 0.1 kubectl get pods -o wide
```

<img width="431" height="668" alt="image" src="https://github.com/user-attachments/assets/c7421aff-6b90-49f8-a982-1318f7f6c1f4" />


### CPU,GPU 중심적인 서버 type=cou 또는 gpu에 pod 생성 
```
nano web-server-deploy.yaml

    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:1
          ports:
            - containerPort: 80
              protocol: TCP
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
              - matchExpressions:
                 - key: type
                   operator: In
                   values:
                    - cpu
                    - gpu
```

```
kubectl apply -y web-server-deploy.yaml            //적용 
watch -n 0.1 kubectl get pods -o wide              //확인 
```

<img width="1075" height="253" alt="image" src="https://github.com/user-attachments/assets/929dde0d-4662-47ad-86c5-997c95e28a02" />




### CPU 중심으로 돌아갈 때

<img width="1097" height="234" alt="image" src="https://github.com/user-attachments/assets/e57bb156-50a2-410f-b9d5-56796bf250ba" />


### GPU 중심으로 돌아갈 때 

<img width="1088" height="232" alt="image" src="https://github.com/user-attachments/assets/81fd0b65-30fa-4ded-a27d-d4af34425879" />




