## Volume 생성 empty 유형

```
$ nano web-server-volume.yaml
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
          image: reallinux/web-server:2
          ports:
            - containerPort: 80
              protocol: TCP
          volumeMounts:
            - name: varlog
              mountPath: /host/var/log
      volumes:
        - name: varlog
          emptyDir: {}
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

#### 변경된 부분

<img width="408" height="152" alt="image" src="https://github.com/user-attachments/assets/ebbc624e-ad1f-47b2-8b55-83af2cd5cc75" />

#### 적용 

```
kubectl apply -f web-server-volume.yaml
kubectl get pods
```
#### Volume 생성

<img width="565" height="225" alt="image" src="https://github.com/user-attachments/assets/76656616-52aa-455d-8a27-e87fca95af25" />

#### Master Node Pod 내부로 들어가기
```
kubectl get pods -o wide         // node의 pods 확인
```

<img width="940" height="221" alt="image" src="https://github.com/user-attachments/assets/809b45a2-1e30-4ebc-abb7-9ea2955ab0f2" />

# web-server pod 내부에서 파일 생성하고 확인하기
```
kubectl exec -it "web-server pod 명" -- bash
cd /host/var/log
ls
touch test.log
exit
```

<img width="939" height="290" alt="image" src="https://github.com/user-attachments/assets/1090bf2c-914c-482d-9ba5-126885ed64ee" />


#### Pod에 재접속 후 확인 
```
kubectl get pods -o wide
kubectl exec -it "web-server pod명" -- bash
cd /host/var/log/
```

<img width="939" height="404" alt="image" src="https://github.com/user-attachments/assets/0220e0e3-3271-4a91-a241-183322987111" />

- 재접속 하여도 이전에 생성했던, test.log 파일이 사라지지 않는 것을 볼 수 있다.

#### Kubectl delete pods "pod 명" 후 확인 
```
kubectl get pods -o wide
kubectl delete pods "pod 명"

kubectl get pods -o wide
kubectl exec -it "새로 생긴 pod명" -- bash

cd /host/var/log
ls
```


<img width="942" height="243" alt="image" src="https://github.com/user-attachments/assets/213925e6-0840-410f-91ed-788913f1e1c0" />


<img width="943" height="370" alt="image" src="https://github.com/user-attachments/assets/6915dd2e-6944-47e3-8110-0757c76fce3c" />

- 삭제하고 다시 들어가도 test.log가 살아있는 것을 확인할 수 있다.

#### 다른 pod로 확인
```
kubectl get pods -o wide
kubectl exec -it "다른 Pods명" -- bash

cd /host/var/log
ls
```

<img width="937" height="374" alt="image" src="https://github.com/user-attachments/assets/c95b27ee-26b2-4f31-a86c-c0e36da08499" />

- 다른 Pod에서도 test.log가 삭제되지 않는 것을 볼 수 있다.

#### kill -9으로 강제종료 후 확인 
```
watch -n 0.1 kubectl get pods -o wide
ps -ef | grep node
kill -9 [pid]

```

<img width="1610" height="225" alt="image" src="https://github.com/user-attachments/assets/90c7b75b-1a45-44c2-9a3a-ae94b73769ce" />
