## Docker의 namespace
### PID namespace 
- 새롭게 PID 1번부터 시작

### Network namespace
- 네트워크 디바이스, port번호, IP 주소등 (독립된 container = 같은 포트번호 사용 가능)

<img width="345" height="371" alt="image" src="https://github.com/user-attachments/assets/3c194d49-485d-4965-9c2b-53b819132c1d" />

### UID namespace
- UID와 GID를 독립적으로 가질 수 있도록 함

### Mount namespace 
- Mount(장치와 폴더의 연결)에 대해서 독립화 시켜 따로 파일시스템 트리를 관리

### UTS namespace
- 호스트명, 도메인명을 독자적으로 가질 수있다.

### IPC namespace
- 프로세스간의 통신에 대해서 namespace 별로 독립화

### User namespace
- 사용자 및 그룹 ID를 격리

---

```
docker inspect [container ID] | grep pid    # PID 얻기
sudo lsns -p [PID]    # namespace 확인
```

