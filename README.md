# Docker
- Inflearn > 개발자를 위한 쉬운 도커



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
- Linux 컨테이너를 기반으로 하여 특정한 서비스(애플리케이션)을 패키징하고 배포하는데 유용한 프로그램이다.
- 애플리케이션 실행에 필요한 모든 것들을 도커 컨테이너에 담아 어떠한 런타임 환경에서도 실행 가능하도록 만들어 주는 프로그램이다.

### 도커 아키텍처
- 도커는 크게 클라이언트와 서버로 구성된다.
  - Client (Docker CLI)
    - Docker daemon과 통신 할 수 있는 명령 도구
    - 사용자가 Docker와 상호작용할 수 있는 가장 우선적인 방법
    - REST API를 통해 Docker daemon에 명령 송신
  - Server (Docker daemon)
    - Docker 이미지를 도커 컨테이너로 만들고 실행하게 해주는 데몬
    - Docker daemon과 통신하거나 Docker API 요청을 기다리고 이미지, 컨테이너, 네트워크, 볼륨 등을 관리하는 역할
    - REST API를 통해 Docker Client와 통신

### 이미지와 컨테이너
- 이미지
  - 컨테이너를 생성하기 위한 파일 시스템과 실행할 애플리케이션의 소스 코드, 라이브러리, 환경 설정 등의 모든 것을 포함하는 템플릿이다.
- 컨테이너
  - 이미지를 기반으로 생성되며, 파일 시스템과 어플리케이션이 구체화되어 실행되는 상태이다.
- 컨테이너의 라이프사이클
  - ![image](https://github.com/Young-Geun/Docker/assets/27760576/bc38eeee-0b0b-4411-835e-0ed54802e9b6)

### 이미지 레지스트리
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

### 이미지 레이어
- 도입배경
  - 도커 이미지는 컨테이너를 생성하기 위한 모든 정보를 갖고 있기 때문에 보통 수백MB ~ 수GB가 넘는다. 그런데 기존 이미지에서 작은 변경사항이 생겨 도커 파일에 코드 한줄을 추가해 다시 이미지를 만들고 그 이미지를 다운로드 받는다고 가정하면, 겨우 코드 한줄 추가했는데 이미지의 불변성 때문에 수백MB ~ 수GB가 되는 이미지를 다시 다운로드 받는 매우 비효율적인 상황이 된다. 이러한 문제를 해결하기 위하여 레이어라는 개념이 도입되었다.
- 레이어란?
  - 기존 이미지에 추가적인 파일이 필요할 때, 다시 다운로드 받는 방법이 아닌 해당 파일을 추가하기 위한 개념이다.
  - ![image](https://github.com/Young-Geun/Docker/assets/27760576/7f085c8d-cda8-456b-900b-dd17f8f01543)
    - Docker 이미지는 위 그림처럼 여러 레이어로 구성되며, 각 레이어는 이전 레이어의 변경 사항을 가지고 있다.
    - 만약 ubuntu 이미지가 기존에 존재하는데, nginx 이미지를 다운 받을 경우 nginx 레이어만 다운받게 된다.
  - 장점
    - 중복 데이터를 최소화할 수 있다.
    - 빌드 속도를 높일 수 있다.
    - 저장소를 효율적으로 사용할 수 있게 해준다.

### 도커 이미지 생성
- 도커 이미지를 생성하는 방법에는 이미지 커밋과 이미지 빌드가 있다.
- 이미지 커밋
  - 도커 컨테이너 내부에서 발생한 변경 사항을 새로운 이미지로 저장하는 기능이다.
  - 이렇게 저장된 이미지는 도커 이미지 레지스트리에 업로드하여 공유하거나 다른 도커 호스트에서 실행할 수 있다.
  - 문제점
    - 이미지를 만들 때마다 컨테이너 실행을 위해 매번 명령어를 수행해야한다.
    - 커밋 하나당 이미지의 레이어가 1개가 추가되므로, 여러 개의 이미지 레이어 추가를 위해서는 여러 번의 커밋이 수행되어야한다.
    - 복잡한 절차로 인해서 실수가 나올 수 있다. (휴먼에러)
- 이미지 빌드
  - 이미지를 만드는 단계를 기재한 명세서인 'Dockerfile'을 통해 이미지를 생성하는 방법이다.
  - 원하는 이미지 상태를 코드로 작성하면, 도커는 이를 읽어들여 컨테이너를 이미지를 생성한다.
  - 커밋 방식의 문제점 때문에 빌드방식을 더 많이 사용한다.

### Dockerfile 지시어
- ![image](https://github.com/Young-Geun/Docker/assets/27760576/d931b008-83ab-4fef-bb76-441ab525f9a7)

### 멀티 스테이지 빌드
- Multi-Stage Build(멀티 스테이지 빌드)란?
  - 단일 도커 파일에서 여러 단계의 빌드를 수행하는 방법이다.
- 사용목적
  - 빌드 도구와 런타임 환경을 분리하고 실행에 필요한 최소한의 구성만 포함하여 이미지 크기를 최소화할 수 있다.
  - 빌드 도구와 관련된 정보들을 외부에 노출시키지 않아 보안을 강화할 수 있다.
  - 중복된 작업을 피하고 이전 단계의 캐시를 활용하기 때문에 빌드 속도가 향상된다.
- 사용예시
  - 단일 스테이지 빌드
    - 실행단계에 불필요한 소스코드 및 빌드단계의 정보가 포함되어 있다.
    - ```
      # 빌드 환경 설정
      FROM maven:3.6-jdk-11 
      WORKDIR /app
      
      # pom.xml과 src/ 디렉토리 복사
      COPY pom.xml .
      COPY src ./src
      
      # 애플리케이션 빌드
      RUN mvn clean package
      
      # 빌드된 JAR 파일을 실행 환경으로 복사
      RUN cp /app/target/*.jar ./app.jar
      
      # 애플리케이션 실행
      EXPOSE 8080
      CMD ["java", "-jar", "app.jar"]
      ```
  - 멀티 스테이지 빌드
    - 실행단계에 필요한 정보만 포함되어 있다.
    - ```
      # 첫번째 단계: 빌드 환경 설정
      FROM maven:3.6 AS build
      WORKDIR /app
      
      # pom.xml과 src/ 디렉토리 복사
      COPY pom.xml .
      COPY src ./src
      
      # 애플리케이션 빌드
      RUN mvn clean package
      
      # 두번째 단계: 실행 환경 설정
      FROM openjdk:11-jre-slim
      WORKDIR /app
      
      # 빌드 단계에서 생성된 JAR 파일 복사
      COPY --from=build /app/target/*.jar ./app.jar
      
      # 애플리케이션 실행
      EXPOSE 8080
      CMD ["java", "-jar", "app.jar"]
      ```

### 볼륨
- 컨테이너의 Stateless
  - 컨테이너는 Stateless한 특징을 가지고 있다.
  - 이 때문에 컨테이너가 삭제되거나 재생성될 경우, 데이터가 모두 초기화된다.
- 볼륨이란?
  - 컨테이너가 삭제되거나 재생성되더라도 데이터를 지속하고 싶은 경우를 위해 등장한 개념이다.
  - 볼륨은 컨테이너와 호스트 운영체제 간에 데이터를 공유하거나 저장하기 위한 기능이다.
  - 볼륨을 사용하면 데이터의 지속성을 유지하면서 컨테이너를 생성하거나 삭제할 수 있다.
- 볼륨 사용의 이점
  - 데이터 지속성
    - 컨테이너가 종료되더라도 볼륨에 저장된 데이터는 유지되기 때문에, 컨테이너 재시작, 업데이트, 삭제 등의 작업으로부터 데이터를 보존할 수 있다.
  - 공유 데이터
    - 여러 컨테이너 간에 데이터를 공유하거나 컨테이너와 호스트 간에 데이터를 주고받을 수 있다.
  - 백업 및 복원
    - 볼륨을 사용하여 중요한 데이터를 백업하고 필요한 경우 복원할 수 있다.

### 레이어 관리
- 레이어 관리의 필요성
  - Dockerfile에 작성된 지시어는 보통의 경우 하나 당 레이어 한 개가 추가된다.
  - 불필요한 레이어가 많아지면 이미지의 크기가 늘어나고 빌드 속도가 느려지기 때문에 레이어 관리가 필요하다.
- 레이어 관리
  - RUN 지시어
    - RUN 지시어는 &&을 활용해 최대한 하나로 처리한다.
    - ![image](https://github.com/Young-Geun/Docker/assets/27760576/2643a5a9-f179-4a72-b412-507684dfcc89)
  - 베이스 이미지
    - 베이스 이미지는 가능한 작은 이미지를 사용한다.
    - Ex) apline OS 사용 권장
  - 불필요한 파일 제거
    - .dockerignore을 사용하여 불필요한 이미지를 제거한다.

### 캐싱을 활용한 빌드
- 캐싱을 위한 배경지식
  - Docker는 각 단계의 결과 레이어를 캐시처리한다.
  - 만약 지시어가 변경되지 않는다면 다음 빌드에서 레이어를 재사용한다.
  - 만약 레이어가 변경된다면, 그 레이어와 이후 모든 레이어는 캐시를 사용하지 않고 새로운 레이어를 만든다.
- 캐싱을 사용하기 위해서는
  - 잘 변경되지 않는 파일들을 아래 레이어에 배치(=Dockerfile에서 먼저 읽히도록 작성한다.)하여 캐시를 활용하는 빈도를 높인다.



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
  - docker pull [이미지명]
  - Ex) docker pull devwikirepo/simple-web:1.0
- 로컬스토리지의 이미지명 추가
  - docker tag [기존 이미지명] [추가할 이미지명]
  - Ex) docker tag devwikirepo/simple-web:1.0 1992choi/my-simple-web:0.1
- 이미지 레지스트리에 이미지 업로드
  - docker push [이미지명]
  - Ex) docker push 1992choi/my-simple-web:0.1

### 이미지 레이어
- 이미지의 레이어 이력 조회
  - docker image history [이미지명]
  - Ex) docker image history devwikirepo/hello-nginx

### 이미지 커밋
- 컨테이너 실행과 동시에 터미널 접속
  - docker run -it --name [컨테이너명] [이미지명] bin/bash
  - Ex) docker run -it --name officialNginx nginx bin/bash
  - 이미지 내부의 파일 시스템을 확인하거나 디버깅 용도로 많이 사용되는 방법
- 실행 중인 컨테이너를 이미지로 생성
  - docker commit -m [커밋명] [실행중인 컨테이너명] [생성할 이미지명]
  - Ex) docker commit -m "edited index.html" -c 'CMD ["nginx","-g","daemon off;"]' officialNginx 1992choi/commitnginx

### 이미지 빌드
- 도커파일을 통해 이미지 빌드
  - docker build -t [이미지명] [Dockerfile경로]
  - Ex) docker build -t 1992choi/buildnginx .
- 도커파일명이 Dockerfile이 아닌 경우
  - docker build -f [도커파일명] -t [이미지명] [Dockerfile경로]
  - Ex) docker build -f Dockerfile-basic -t buildapp:basic .

### 애플리케이션 구성
- 네트워크 생성
  - docker network create leafy-network
- DB 구성 및 확인
  - docker run -d --name leafy-postgres --network leafy-network devwikirepo/leafy-postgres:1.0.0
  - docker logs -f leafy-postgres
- Backend 구성 및 확인
  - docker run -d -p 8080:8080 -e DB_URL=leafy-postgres --network leafy-network --name leafy devwikirepo/leafy-backend:1.0.0
  - docker logs -f leafy
- Frontend 구성 및 확인
  - docker run -d -p 80:80 --network leafy-network --name leafy-front devwikirepo/leafy-frontend:1.0.0
  - docker logs -f leafy-front
- 접속정보
  - john123@qmail.com / password123

### 애플리케이션 구성 (직접 구성)
- leafy 디렉토리 기반으로 구성
- 네트워크 생성
  - docker network create leafy-network
- DB 구성 및 확인
  - leafy/leafy-postgresql 디렉토리에서 수행
  - docker build -t 1992choi/leafy-postgres:1.0.0 .
  - docker push 1992choi/leafy-postgres:1.0.0
  - docker run -d --name leafy-postgres --network leafy-network 1992choi/leafy-postgres:1.0.0
  - docker logs -f leafy-postgres
- Backend 구성 및 확인
  - leafy/leafy-postgresql 디렉토리에서 수행
  - docker build -t 1992choi/leafy-backend:1.0.0 .
  - docker push 1992choi/leafy-backend:1.0.0
  - docker run -d -p 8080:8080 -e DB_URL=leafy-postgres --network leafy-network --name leafy 1992choi/leafy-backend:1.0.0
  - docker logs -f leafy
  - curl http://localhost:8080/api/v1/users
- Frontend 구성 및 확인
  - leafy/leafy-frontend 디렉토리에서 수행
  - docker build -t 1992choi/leafy-frontend:1.0.0 .
  - docker push 1992choi/leafy-frontend:1.0.0
  - docker run -d -p 80:80 --network leafy-network --name leafy-front 1992choi/leafy-frontend:1.0.0

### 네트워크
- 네트워크 리스트 조회
  - docker network ls
- 네트워크 상세 정보 조회
  - docker network inspect [네트워크명]
  - Ex) docker network inspect leafy-network
- 네트워크 생성
  - docker network create [네트워크명]
  - Ex) docker network create new-network
- 네트워크 삭제
  - docker network rm [네트워크명]
  - Ex) docker network rm new-network
- 포트 포워딩
  - docker run -p [Host OS의 포트]:[컨테이너의 포트]

### 볼륨
- 볼륨 리스트 조회
  - docker volume ls 
- 볼륨 상세 정보 조회
  - docker volume inspect [볼륨명]
- 볼륨 생성
  - docker volume create [볼륨명]
- 볼륨 삭제
  - docker volume rm [볼륨명]
