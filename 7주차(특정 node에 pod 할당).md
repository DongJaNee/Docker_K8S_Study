## Master노드와 Worker노드에 각각 라벨 설정 
```
kubectl label node <name> type=cpu      //서버가 CPU 특화 되어있다는 의미의 라벨 (마스터 노드 기준으로 라벨 설정)
kubectl describe node <name> | less

kubectl label node <name> type=gpu      // 서버가 GPU 특화 되어있다는 의미의 라벨 (Worker 노드 기준으로 라벨 설정)
kubectl describe node <name> | less 
