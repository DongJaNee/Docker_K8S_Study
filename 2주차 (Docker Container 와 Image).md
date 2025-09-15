## Docker Container와 Image 다루기 
- Docker의 이미지는 프로그램과 같다
```
프로그램 -> 프로세스 1
            프로세스 2

이미지 -> 컨테이너 1
         컨테이너 2
```
- 하지만 차이점이 있다면 이미지는 Linux환경 / root file system 등을 포함한다

---

## Docker Container 여러개 Stop/Start/Remove

<img width="854" height="94" alt="image" src="https://github.com/user-attachments/assets/f9321153-7a4e-4775-9cc8-29049983a91d" />

### Docker Container Stop 

```
docker stop $(docker ps -q)
```

<img width="971" height="274" alt="image" src="https://github.com/user-attachments/assets/e9549e03-e18e-4337-a332-39eac8215eab" />


### Docker Container Start

```
docker start $(docker ps -aq)
```

<img width="933" height="292" alt="image" src="https://github.com/user-attachments/assets/fb678b1d-9dac-4d60-9547-8b547b3205d1" />

### Docker Container Remove

```
docker rm $(docker ps -aq)
```

<img width="1032" height="227" alt="image" src="https://github.com/user-attachments/assets/241410e8-2901-4e96-8971-fc3fe9cdd02e" />

---

## Docker diff : 변경점 확인 
- diff 는 원본 이미지와의 차이점으로 변경점을 적용 한다.




```
docker attach [Container ID]    # Container로 접속
mkdir abc      # abc 디렉터리를 만들기 
docker diff [Container ID]    # 변경점 확인하기 

A : Added
C : Changed
D : Deleted
```

<img width="920" height="277" alt="image" src="https://github.com/user-attachments/assets/b81ea584-44b0-4770-a457-115995f1e854" />

---

## Docker Commit으로 새로운 이미지 생성 
- docker container의 변경점된 내용으로 이미지에 저장하고 싶을 때 

```
docker commit [Container ID] [생성 할 이미지 이름]
ex) docker commit asdfasdfasdf ubuntu-test
```


<img width="599" height="223" alt="image" src="https://github.com/user-attachments/assets/15afb94f-8062-442e-a07d-2021dce732b8" />

---

<img width="878" height="162" alt="image" src="https://github.com/user-attachments/assets/eab9b0ff-cde4-4f00-9a2b-7d3e158ea5d1" />

- 들어가서 실행해보면 mkdir로 생성했었던 abc 디렉터리를 볼 수 있다.

