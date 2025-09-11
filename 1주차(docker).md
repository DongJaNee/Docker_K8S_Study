# Setup Docker apt repository 

Docker : https://docs.docker.com/engine/install/ubuntu/

```
# Add Docker's official GPG key:
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl
$ sudo install -m 0755 -d /etc/apt/keyrings
$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
$ sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt-get update

```

<br>

```
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# docker 그룹에 worknode1 추가
$ sudo usermod -aG docker worknode1

$ sudo reboot 
```

# docker container 생성하고 내부로 들어가기 
```
# ubuntu:16.04 Test
$ docker run --rm -it ubuntu:16.04 /bin/bash
```

PID 변경 확인

<img width="979" height="116" alt="image" src="https://github.com/user-attachments/assets/34cc49e4-5790-46ac-816a-72d03e6bb4e1" />
