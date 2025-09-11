## 컨테이너란?
- 독립된 리눅스 환경을 보장받는 프로세스 (**namespace**, **cgroup**을 적용한)

<img width="407" height="483" alt="image" src="https://github.com/user-attachments/assets/b6adbfb6-6237-407a-963b-5ee304461bb3" />


- #### 컨테이너 타입 
1) App 컨테이너
2) 머신 컨테이너

- ### namespace
   **Application 관리** : 커널자원 (ex : PID, port, rootfs, ...)

   **어디까지 볼수있는지 (see)**  
- ### cgroup
   **Hardware(CPU, memory, network, disk) 자원관리**

  **얼마나 쓸수있는지(use)**

