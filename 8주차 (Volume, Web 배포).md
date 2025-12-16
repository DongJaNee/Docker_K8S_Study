
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



