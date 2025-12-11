## Ingress 
- 쿠버네티스 클러스터 외부에서 내부 서비스로 HTTP/HTTPS 트래픽을 라우팅하는 규칙들의 집합

<img width="1249" height="650" alt="image" src="https://github.com/user-attachments/assets/086f1f25-473a-438f-9c5b-ef66c41fc4b3" />

## 싱글 Service 기준 Ingress 생성

<img width="614" height="573" alt="image" src="https://github.com/user-attachments/assets/bf6259ff-4282-4dcc-8f2b-7b06f88d1ea1" />

## Ingress Test
```
$ nano /etc/hosts            //자기 자신의 IP 주소로 host 생성
```

<img width="529" height="150" alt="image" src="https://github.com/user-attachments/assets/dd3a8608-d63d-4516-be67-bca48807aa05" />

```
$ nano web-server-ingress.yaml

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

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
    - host: reallinux.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web-server
                port:
                  number: 8080

```
```  
$ kubectl get ingress
$ kubectl describe ingress

# 접속테스트
$ curl reallinux.io

#종료
$ kubectl delete -f web-server-ingress.yaml
```

<img width="744" height="290" alt="image" src="https://github.com/user-attachments/assets/a84cf4f3-e435-44ca-9c8b-4154b514e45a" />

---
 
 <img width="446" height="214" alt="image" src="https://github.com/user-attachments/assets/182a2d27-f9e8-40a0-a0d9-408d3a130441" />

 - **로드밸런싱된 것을 확인할 수 있다.**

## 멀티 Service 기준 Ingress 생성 
```
# YAML 파일 생성 후
$ nano web-server-ingress2.yaml

# 생성
$ kubectl apply -f web-server-ingress2.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server-rs-1
spec:
  replicas: 5
  selector:
    matchLabels:
      app: webapp-1
      tier: app-1
  template:
    metadata:
      labels:
        app: webapp-1
        tier: app-1
    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:2
          ports:
            - containerPort: 80
              protocol: TCP

---
apiVersion: v1
kind: Service
metadata:
  name: web-server-1
spec:
  type: ClusterIP
  ports:
    - port: 8080
      targetPort: 80
      protocol: TCP
  selector:
      app: webapp-1
      tier: app-1

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-server-rs-2
spec:
  replicas: 5
  selector:
    matchLabels:
      app: webapp-2
      tier: app-2
  template:
    metadata:
      labels:
        app: webapp-2
        tier: app-2
    spec:
      containers:
        - name: web-server
          image: reallinux/web-server:3
          ports:
            - containerPort: 80
              protocol: TCP
---
apiVersion: v1
kind: Service
metadata:
  name: web-server-2
spec:
  type: ClusterIP
  ports:
    - port: 8080
      targetPort: 80
      protocol: TCP
  selector:
      app: webapp-2
      tier: app-2

---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
    - host: reallinux.io
      http:
        paths:
          - path: /hello
            pathType: Prefix
            backend:
              service:
                name: web-server-1
                port:
                  number: 8080
          - path: /world
            pathType: Prefix
            backend:
              service:
                name: web-server-2
                port:
                  number: 8080

```

<img width="408" height="421" alt="image" src="https://github.com/user-attachments/assets/e3ada6b4-93f7-44c5-8dd1-c553b68cd407" />

```
# 확인
kubectl get all
```


<img width="724" height="582" alt="image" src="https://github.com/user-attachments/assets/194e6aea-3502-4565-b21a-1351b3d6a154" />

```
# 테스트
$ curl reallinux.io/hello
$ curl reallinux.io/world

# 종료
$ kubectl delete -f web-server-ingress2.yaml 
```


<img width="489" height="310" alt="image" src="https://github.com/user-attachments/assets/73ae1a63-1afc-4ab0-8a67-a052578a9b37" />

---
## DNS Test 
```
# yaml 파일 수정하기
# 예시 도메인 : 192-168-0-167.nip.io            //사용하고있는 local 주소를 기입해야함.
$ kubectl delete -f web-server-ingress2.yaml         //기존에 사용중인 ingress2 삭제
$ nano web-server-ingress.yaml
```



<img width="350" height="206" alt="image" src="https://github.com/user-attachments/assets/1e86ff97-2fd5-4a21-bb0c-0647731586b1" />

```
# Spec 부분에 있는 rules의 host 부분에 도메인을 입력
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
    - host: 192-168-0-167.nip.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web-server
                port:
                  number: 8080
```

```
# 오브젝트 스펙 yaml 적용
$ kubectl apply -f web-server-ingress.yaml

# 웹 요청 테스트
curl 192-168-0-167.nip.io
```


<img width="471" height="165" alt="image" src="https://github.com/user-attachments/assets/1ba62bd8-4da4-4427-8da9-4c9c24cb5dd8" />

