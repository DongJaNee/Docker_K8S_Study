## Docker Hub 계정 만들기 

<img width="1910" height="963" alt="image" src="https://github.com/user-attachments/assets/38c7ceae-fcd1-40cb-8cb3-97bdd9038a31" />

https://hub.docker.com

### docker login 명령으로 로그인 정보 저장하기 
```
docker login 또는 docker login u
```

<img width="867" height="50" alt="image" src="https://github.com/user-attachments/assets/98ac4f16-e774-4a4b-9010-de5a267c9f0c" />


<img width="736" height="132" alt="image" src="https://github.com/user-attachments/assets/095287fa-01a0-4ae6-af1a-0137708edf6e" />


## docker push로 이미지 올리기 
### docker 컨테이너 생성 
```
docker run --name ubuntu-test -it ubuntu:16.04    
```

<img width="891" height="198" alt="image" src="https://github.com/user-attachments/assets/4ec38b81-d9f1-4011-91de-20a073b25ff6" />

---
### docker 이미지 생성 
```
docker commit ubuntu-test ubuntu-test
```

<img width="896" height="267" alt="image" src="https://github.com/user-attachments/assets/449e7de6-c64e-4470-8713-34e9c000a279" />

---
### docker push로 이미지 올리기 

```
docker tag ubuntu-test mvsark/ubuntu-test:1
docker push mvsark/ubuntu-test:1
```


<img width="767" height="165" alt="image" src="https://github.com/user-attachments/assets/fcb8f181-285e-4fe4-8a71-6873a1f83868" />

---
## Dockerfile로 이미지 파일 Build 
### Dockerfile 구성요소 
- **From** : 베이스 이미지 정보
- **RUN** : 명령어 실행
- WORKDIR<작업폴거 경로> : RUN, CMD   등이 이루어질 기본 디렉터리 경로 
- ARG : 변수값 설정
- ENV : 환경변수값 설정
- EXPOSE : 도커가 실행되었을때 사용할 port 번호
- CMD : 컨테이너 실행 default 명령

## 오픈소스 프로젝트 uftrace 환경구축 Dockerfile 
### uftrace 오픈소스 다운 
```
git clone https://github.com/namhyung/uftrace.git
```

### uftrace 폴더로 이동
```
cd uftrace
```

<img width="1061" height="63" alt="image" src="https://github.com/user-attachments/assets/1a4bdd98-5b5a-43bd-9a0d-d5614afa493b" />

---

### uftrace 오픈소스에서 Dockerfile 찾기 
```
find ./ -name Dockerfile
```

<img width="527" height="168" alt="image" src="https://github.com/user-attachments/assets/a932310b-9a97-4f9c-b3b8-ade1e2bf4769" />

---

### uftrace Dockerfile을 통해서 리눅스 환경 이미지 만들기
```
cd misc/docker/ubuntu/18.04
docker build .
```

## 오픈소스 프로젝트 pytorch 환경구축 Dockerfile 
### pytorch 오픈소스 다운받기

```
git clone https://github.com/pytorch/pytorch.git
```

### pytorch 폴더로 이동
```
cd pytorch
```


<img width="1007" height="68" alt="image" src="https://github.com/user-attachments/assets/2c0231c2-9b6d-480e-80a2-5039dff471a8" />

---

### pytorch Dockerfile을 통해서 리눅스 환경 이미지 만들기 
```
docker build --tag pytorch_test .
```
