# namespace test

Test할 filesystem (rootfs)

<img width="640" height="49" alt="image" src="https://github.com/user-attachments/assets/94225cdf-a136-4c5c-bce7-ca51b123ca8d" />


```
# 현재 시스템에서 모든 프로세스 확인 
$ ps -ef 
```
<img width="641" height="206" alt="image" src="https://github.com/user-attachments/assets/ababfa1f-804e-4639-80c9-fdacd0378508" />


```
# 루트 파일시스템만 독립화 시킨 
$ sudo chroot rootfs /bin/bash

# proc 연결
$ mount -t proc proc /proc

# 모든 프로세스 확인
$ ps -ef

# 프로세스 관리 (proc) 독립화 + 루트 파일시스템 독립화
$ sudo unshare -p -f --mount-proc=rootfs/proc chroot rootfs /bin/bash

# 프로세스 PID 1번 확인하기
ps -ef
```

<img width="641" height="191" alt="image" src="https://github.com/user-attachments/assets/4925fadd-b901-4b3c-82c9-b15b9a9deff2" />

# cgroup test

```
# root로 접속하여 새로운 cgroup "test" 생성하기 

$ sudo su

$ mkdir /sys/fs/cgroup/memory/test

# 새로운 cgroup "test" 메모리 100MB로 제한
$ echo 100000000 > /sys/fs/cgroup/memory/test/memory.limit_in_bytes

# swap 메모리 0으로 swap안쓰기
$ echo 0 > /sys/fs/cgroup/memory/test/memory.swappiness

# 현재 실행중인 PID 에 적용하기
$ echo $$ > /sys/fs/cgroup/memory/test/tasks


# 10MB씩 갉아먹는 python Test Text 만들기
$ nano mem_eater.py
f = open("/dev/urandom", "r")
data = ""

i=0
while True:
  data += f.read(10000000)
  i += 1
  print "%dmb" % (i*10,)

# mem_eater.py 실행
$ python mem_eater.py

```

<img width="466" height="173" alt="image" src="https://github.com/user-attachments/assets/82a8796e-7205-4a0d-9930-2ff11d1d06fa" />
