## Ingress 
- 쿠버네티스 클러스터 외부에서 내부 서비스로 HTTP/HTTPS 트래픽을 라우팅하는 규칙들의 집합

<img width="1249" height="650" alt="image" src="https://github.com/user-attachments/assets/086f1f25-473a-438f-9c5b-ef66c41fc4b3" />

### 싱글 Service 기준 Ingress 생성

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

