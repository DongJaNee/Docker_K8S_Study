## Docker를 통한 cgroup (cpu, cpuacct) : Process Controll Group
- CPU 사용률 제한

### 컨테이너에 접속하기

```
docker attach [Container ID]    # Container에 접속하기
```

<img width="923" height="130" alt="image" src="https://github.com/user-attachments/assets/816fd0c6-2682-447a-ac8f-436240c56a94" />


### 기존 컨테이너에서 cgroup 설정값 변경 (Update)
```
docker update --cpu-quota=50000 [Container ID]
```

<img width="620" height="33" alt="image" src="https://github.com/user-attachments/assets/fb505b9c-c712-4d30-b9e0-e2d300f4b262" />


<img width="558" height="34" alt="image" src="https://github.com/user-attachments/assets/970530b8-54bb-4065-9a52-941bbe414439" />


### 컨테이너를 생성하면서 설정값 적용 
```
docker run --cpu-period=1000000 --cpu-quota=500000 ubuntu:18.04 /bin/bash
```

### Docker Container 디렉터리에 직접 접속해서 echo를 사용해서 적용

```
# sudo su 를 사용해서 root로 접속한 뒤 
cd /sys/fs/cgroup/cpu.cpuacct/docker
echo [변경할 cpu용량] > cpu.cfs_quota_us
echo [기준 cpu용량] > cpu.cfs_period_us
```

<img width="769" height="97" alt="image" src="https://github.com/user-attachments/assets/3321018c-c423-4b7b-add9-f97a6dfea844" />

---

## Docker를 통한 cgroup (cpuset) : CPU 코어제한 

```
nproc # 코어확인
```

<img width="269" height="32" alt="image" src="https://github.com/user-attachments/assets/5a28e50d-666d-44fb-b9d0-67c6179377f1" />

```
docker run --cpuset-cpus=0,1 -it ubuntu:18.04 /bin/bash   # CPU 코어 2개로 제한
```

---

## Docker를 통한 cgroup : Memory 사용률 % 제한 

```
docker run --memory=100M --memory-swappiness 0 -it ubuntu:18.04 /bin/bash
```

**※ 사용법은 cpuacct와 동일하다**


