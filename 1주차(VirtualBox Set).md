# Oracle VirtualBox Set
## 1. Oracle VirtualBox Download
1. Windows 환경에서 VM을 실행할 Oracle VirtualBox를 다운로드 한다.

※  주소 : https://www.virtualbox.org/wiki/Downloads

2. 접속한 URL에서 Windows hosts를 다운받으면 된다. 

<img width="577" height="284" alt="image" src="https://github.com/user-attachments/assets/ee0088f0-6298-421b-a5b3-19f9abc6109e" />

## 2. Oracle VirtualBox에서 VM 생성
1. 미리 다운로드 받아놓은 OS를 가져온다.
1) 새로만들기 -> 이미지 파일 설정 -> VM 이름 및 비밀번호 생성 

<img width="701" height="486" alt="image" src="https://github.com/user-attachments/assets/b7eed545-d4b2-428f-99cc-1288f2dd7ab9" />

<img width="761" height="542" alt="image" src="https://github.com/user-attachments/assets/8a9fafa1-1927-4e7f-9de0-c7bf8e385649" />

2. 포트포워딩 설정
1) 접속한 VM에 ifconfig / ip addr show 명령어를 입력해서 ip를 확인 

<img width="800" height="310" alt="image" src="https://github.com/user-attachments/assets/03698331-f711-4ffd-a31c-0351cc42ae06" />

2) 생성한 VM을 우클릭 설정 -> 네트워크 -> 포트포워딩 

<img width="880" height="602" alt="image" src="https://github.com/user-attachments/assets/32d8d2dc-3185-426f-a569-1613c18324e8" />

3) 포트 포워딩 규칙에서 IP와 사용할 Port를 입력

<img width="882" height="605" alt="image" src="https://github.com/user-attachments/assets/cc7e9ae8-0b4d-4bd0-b8e2-5af02e6611fc" />

## 3. PuTTy로 접속하기 
1. PuTTy 실행 

<img width="589" height="536" alt="image" src="https://github.com/user-attachments/assets/4700425d-fd60-4d32-a2bd-b28a3e630746" />

2. HostName 입력란에 VM 커널에서 사용하는 UserName과 IP Host IP 주소 입력 
ex) worknode1@127.0.0.1

<img width="593" height="535" alt="image" src="https://github.com/user-attachments/assets/7983f32b-0305-461d-8974-1225f9ccea2d" />

3. 포트포워딩에서 설정했던 Port번호 입력

4. PuTTy 접속화면

<img width="656" height="412" alt="image" src="https://github.com/user-attachments/assets/7bd9401b-2563-42c7-b7bd-549901f17f87" />
