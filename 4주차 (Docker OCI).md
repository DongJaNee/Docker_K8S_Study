# OCI(Open Container Initative)
## 표준 동작 (Standard Operations)
- 컨테이너 생성, 시작, 정지 등 기본 동작이 **표준 도구**로 가능해야 함.
-  파일 시스템 도구를 통해 **스냅샷, 복사**가 가능해야 함.
-  네트워크 도구를 통해 **업로드, 다운로드**가 가능해야 함.

## 내용 중립성 (Content-agnostic)
- 표준 컨테이너는 컨테이너가 담고 있는 애플리케이션의 종류에 상관없이 **표준 동작들이 동일하게 동작**해야 함.
ex) Web서버든 DB서버든 동일한 컨테이너 동작을 보장.

## 인프라 중립성 (Infrastructure-Agnostic)
- 다양한 인프라(클라우드, 온프레미스 등)에서 **동일하게 실행 가능**
- OCI를 지원하는 환경이라면 어디서든 컨테이너가 작동해야 함.

## 자동화를 위한 설계 (Designed for Automation)
- 표준화된 동작 덕분에 **자동화 도구와 쉽게 통합** 가능
- DevOps, CI/CD 파이프라인에 적합

## 산업 수준의 배포 (Industry-Grade Delivery)
- **기업 규모와 관계없이** 안정적이고 확장 가능한 배포가 가능해야 함.
- 대규모 서비스 운영에도 적합한 품질과 신뢰성 제공

# OCI 표준의 주요 구성요소

| 구성 요소            | 설명                                                                 |
|---------------------|----------------------------------------------------------------------|
| **image-spec**       | 컨테이너 이미지의 디스크 포맷을 정의. 이미지 구조와 메타데이터 포함.     |
| **runtime-spec**     | 컨테이너의 실행 환경과 설정, 라이프사이클을 명시. 런타임 동작의 기준.       |
| **distribution-spec**| 이미지 및 아티팩트를 다양한 레지스트리에서 저장·전송하기 위한 API 표준.     |
| **runc**             | OCI runtime-spec을 구현한 CLI 도구. 실제 컨테이너를 생성하고 실행함.       |
| **image-tools**      | image-spec 기반 이미지 생성 및 검증 도구 모음.                            |
| **runtime-tools**    | runtime-spec 기반 런타임 테스트 및 검증 도구 모음.                         |
| **go-digest**        | 콘텐츠 주소 지정용 digest 패키지. 이미지 무결성 검증에 사용됨.              |
| **selinux**          | 컨테이너 보안을 위한 SELinux 설정 및 정책 지원.                            |


## runc
- runc는 OCI를 구현한 경량 컨테이너 런타임
- 컨테이너를 실제로 생성하고 실행하는 도구

### runc의 역할
- 컨테이너 프로세스 생성
- **namespace** 격리
- **cgroups** 설정

```
Kubernetes (Orchestrator)
   ↓
CRI (Container Runtime Interface)
   ↓
containerd / CRI-O (High-level Runtime)
   ↓
runc (Low-level Runtime)
   ↓
Linux Kernel (Namespaces, cgroups, etc.)
```


<img width="294" height="310" alt="image" src="https://github.com/user-attachments/assets/e986882d-5946-4f08-b8f6-c7c1a48a8929" />


## runtime
- 이 프로그램을 실행할 때 어떤 도구를 쓸 건지
  
