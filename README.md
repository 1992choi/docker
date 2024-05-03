# Docker

<br><br>

## 개념 정리
### 서버
- 서버란?
  - 서버(server)의 사전적 의미 자체는 ‘서비스 (service)를 제공하는 사람’이라는 뜻이다.
  - 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템
  - 하드웨어 뿐만 아니라 소프트웨어까지 아우르는 개념
- 서버의 종류
  - 파일서버
  - DB 서버
  - 웹 서버
  - 웹 애플리케이션 서버
  - 등등...
- 엔터프라이즈 서버 운영 방식
  - 베어메탈
  - 하이퍼바이저
  - 컨테이너
- 하이퍼바이저와 컨테이너
  - 하이퍼바이저
    - 하나의 컴퓨터에서 다수의 독립적인 OS를 운영한다.
    - Host OS 위에 다수의 Guest OS를 가상화하여 사용하는 방식이다.
    - 대표적으로 VMware, VirtualBox가 존재한다.
  - 컨테이너
    - 추가적인 Guest OS를 설치하여 가상화하는 방식은 성능면에서 이슈가 있고, 이를 개선하기 위해 프로세스를 격리된 환경에서 실행하는 기술, 즉 컨테이너 가상화 기술이 등장했다.
    - 운영체제 수준의 가상화 기술로 리눅스 커널을 공유함과 동시에 프로세스를 격리된 환경에서 실행한다.
    - 때문에 하이퍼바이저 가상화 방식보다 더욱 가볍고 빠르게 동작한다.
### 도커
- 도커란?
  - Linux 컨테이너를 기반으로 하여 특정한 서비스(애플리케이션)을 패키징하고 배포하는데 유용한 프로그램이다.
  - 애플리케이션 실행에 필요한 모든 것들을 도커 컨테이너에 담아 어떠한 런타임 환경에서도 실행 가능하도록 만들어 주는 프로그램이다.
- 도커 아키텍처
  - 도커는 크게 클라이언트와 서버로 구성된다.
  - Client (Docker CLI)
    - Docker daemon과 통신 할 수 있는 명령 도구
    - 사용자가 Docker와 상호작용할 수 있는 가장 우선적인 방법
    - REST API를 통해 Docker daemon에 명령 송신
  - Server (Docker daemon)
    - Docker 이미지를 도커 컨테이너로 만들고 실행하게 해주는 데몬
    - Docker daemon과 통신하거나 Docker API 요청을 기다리고 이미지, 컨테이너, 네트워크, 볼륨 등을 관리하는 역할
    - REST API를 통해 Docker Client와 통신
- 이미지와 컨테이너
  - 이미지
    - 컨테이너를 생성하기 위한 파일 시스템과 실행할 애플리케이션의 소스 코드, 라이브러리, 환경 설정 등의 모든 것을 포함하는 템플릿이다.
  - 컨테이너
    - 이미지를 기반으로 생성되며, 파일 시스템과 어플리케이션이 구체화되어 실행되는 상태이다.
- 컨테이너의 라이프사이클
  - ![image](https://github.com/Young-Geun/Docker/assets/27760576/bc38eeee-0b0b-4411-835e-0ed54802e9b6)
- 이미지 레지스트리
  - 이미지 레지스트리란?
    - 도커 이미지를 관리하는 일종의 저장소이다.
  - 제공 기능
    - 이미지를 업로드 / 다운로드
    - 이미지를 검색하는 기능
    - 이미지 버전 관리
    - 파이프라인
  - 이미지명 규칙
    - 레지스트리 주소/프로젝트명/이미지명:이미지태그
    - Ex) docker.io/library/nginx:latest

<br><hr><br>

## 실습
### 컨테이너 실행
- Client, Server의 버전 및 상태 확인
  - docker version
- 플러그인, 시스템 상세 정보 확인
  - docker info
- 도움말
  - docker --help
- 컨테이너 실행
  - docker run [실행옵션] [이미지명]
  - Ex) docker run -p 80:80 --name hellonginx nginx
- 컨테이너 삭제
  - docker rm [컨테이너명/ID]
  - Ex) docker rm hellonginx
- 실행중인 컨테이너 리스트 조회
  - docker ps
- 실행중인 컨테이너의 로그 조회
  - docker logs [컨테이너명]
### 이미지
- 이미지 조회
  - docker image ls
- 특정 이미지 조회
  - docker image ls [이미지명]
  - Ex) docker image ls nginx
- 이미지 삭제
  - docker image rm [이미지명]
### 이미지의 메타데이터
- 이미지의 세부 정보 조회
  - docker image inspect [이미지명]
- 컨테이너의 세부 정보 조회
  - docker container inspect [컨테이너명]
- 컨테이너 실행 시 메타데이터의 env 덮어쓰기
  - docker run --env KEY=VALUE [이미지명]
### 이미지 레지스트리
- 로컬 스토리지로 이미지 다운로드
  - docker pull 이미지명
  - Ex) docker pull devwikirepo/simple-web:1.0
- 로컬스토리지의 이미지명 추가
  - docker tag 기존이미지명 추가할이미지명
  - Ex) docker tag devwikirepo/simple-web:1.0 1992choi/my-simple-web:0.1
- 이미지 레지스트리에 이미지 업로드
  - docker push 이미지명
  - Ex) docker push 1992choi/my-simple-web:0.1
