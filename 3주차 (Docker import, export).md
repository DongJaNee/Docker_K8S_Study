## docker save를 통해서 이미지를 tar파일로 저장하기
- 기존에 있던 이미지를 기준으로 tar파일로 만든다.

```
mkdir ~/docker_tar    # docker_tar디렉터리 생성

cd docker_tar    # docker_tar파일로 이동

docker save ubuntu:16.04 -o ubuntu.tar    # 이미지 파일 저장
```


<img width="652" height="84" alt="image" src="https://github.com/user-attachments/assets/96cd0adf-a775-4b24-afe0-6c6f018980b4" />



<img width="615" height="84" alt="image" src="https://github.com/user-attachments/assets/13fb8e32-1ccb-4184-89bf-da43bc3e33c5" />


```
file ubuntu.tar    # tar 파일 확인

mkdir ubuntu      # ubuntu 디렉터리 생성

tar -C ubuntu -xvf ubuntu.tar      # ubuntu 파일에 ubuntu.tar로 압축 풀기

tree ubuntu ./      # tree로 압축 푼 ubuntu 파일 확인

```


<img width="702" height="503" alt="image" src="https://github.com/user-attachments/assets/b0a0597b-d997-4ccd-ba94-8456691e0ab6" />


---

<img width="439" height="149" alt="image" src="https://github.com/user-attachments/assets/0257dfbb-e1af-40df-91a2-e838e583bf3d" />


## load를 통해서 tar파일 image파일로 load

```
docker load -i ubuntu.tar
```


<img width="811" height="199" alt="image" src="https://github.com/user-attachments/assets/4a889907-d335-47e0-ace8-41dc0fa8a867" />



## docker export를 통해서 컨테이너의 filesystem tar 파일로 저장하기
- export는 컨테이너의 root file system 기준으로 만든다.

```
cd ~/docker_tar     # docker_tar 파일로 이동

docker container list -a | grep ubuntu | head -1        # 첫번째 컨테이너 ID 얻어오기

docker export [Container ID] -o ubuntu_fs.tar    # docker container ID로 export하기

file ubuntu_fs.tar      # file 확인하기

mkdir ubuntu_fs      # ubuntu_fs 디렉터리 생성
tar -C ubuntu_fs -xvf ubuntu_fs.tar        # 생성한 ubuntu_fs에 ubuntu_fs.tar이름으로 압축 풀기 
```


<img width="848" height="214" alt="image" src="https://github.com/user-attachments/assets/8f1e8d5c-c4a0-4e2d-a30f-f5f57d999761" />


---

```
tree -L 1 ubuntu_fs        # tree 명령어로 확인
```


<img width="494" height="371" alt="image" src="https://github.com/user-attachments/assets/f8d3a157-b7dc-4d9a-81b4-0b347440c5ea" />


## docker import를 통해서 tar파일을 이미지로 만들기
- docker import는 root file system으로 tar파일로 만든 것 기준으로 import해야한다.
- root file system -> 이미지화 시킴

```
docker import ubuntu_fs.tar import:16.04      # ubuntu:16.04에 ubuntu_fs.tar파일을 이미지화 시키기

docker images        # docker 이미지 확인
```


<img width="633" height="116" alt="image" src="https://github.com/user-attachments/assets/e71360d1-eb08-4a8e-a53c-ac79dfda83f0" />
