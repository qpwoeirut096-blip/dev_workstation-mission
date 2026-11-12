# Docker 웹 컨테이너 프로젝트

## 1. 프로젝트 소개
이 프로젝트는 Docker를 사용하여 정적 웹페이지를 컨테이너 환경에서 실행하는 실습 프로젝트이다.  
`app/index.html` 파일에 작성한 HTML 페이지를 Docker 이미지로 빌드하고, 컨테이너로 실행한 뒤 브라우저에서 정상적으로 접속되는지 확인하였다.  
또한 Docker 설치 확인, `hello-world` 실행, 이미지 및 컨테이너 목록 확인, 포트 매핑, Git 설정 확인, 볼륨 마운트와 데이터 영속성 확인까지 함께 수행하였다.

---

## 2. 개발 및 실행 환경
- 운영체제: Windows
- 터미널: Windows CMD
- Docker Desktop: [직접 입력]
- Git: [직접 입력]
- 웹 브라우저: Chrome 또는 Edge
- 사용 포트: `8080`

### 2-1. 터미널 환경
본 실습은 `Windows CMD` 환경에서 진행하였다.

### 2-2. Docker 버전 확인
```bash
docker version
```

```text
[Client:
 Version:           29.9.2
 API version:       1.56
 Go version:        go1.26.10
 Git commit:        a82e528
 Built:             Tue Oct 20 19:44:07 2026
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.97.0 (244584)
 Engine:
  Version:          29.9.2
  API version:      1.56 (minimum version 1.40)
  Go version:       go1.26.10
  Git commit:       0367cbe
  Built:            Tue Oct 20 18:45:02 2026
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v2.4.2
  GitCommit:        b7a25261e029d68978bbf290df0a62fc819b3c51
 runc:
  Version:          1.5.2
  GitCommit:        v1.5.2-0-g29dd3dc2
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0]
```

### 2-3. Docker 데몬 상태 확인
```bash
docker info
```

```text
[docker info 명령어 실행 결과, Server 정보가 정상적으로 출력되어 Docker Desktop 데몬이 실행 중임을 확인하였다.]
```

### 2-4. Git 버전 확인
```bash
git --version
```

```text
[git version 2.56.0.windows.4]
```

---

## 3. 프로젝트 구성
```text
project-root/
├─ Dockerfile
├─ README.md
├─ app/
│  └─ index.html
└─ docs/
   └─ evidence/
      ├─ docker-version.png
      ├─ docker-info1.png
      ├─ docker-info2.png
      ├─ docker-images.png
      ├─ docker-ps-a.png
      ├─ docker-hello-world.png
      ├─ docker-build.png
      ├─ docker-run-web-test.png
      ├─ localhost-8080.png
      ├─ git-version.png
      ├─ git-config.png
      ├─ git-remote-v.png
      ├─ chmod-ls-l.png
      ├─ volume-run.png
      └─ volume-after-delete.png
```

### 3-1. 파일 및 폴더 설명
- `Dockerfile` : Docker 이미지를 생성하기 위한 설정 파일
- `README.md` : 프로젝트 수행 과정과 결과를 정리한 문서
- `app/index.html` : 컨테이너에서 서비스할 정적 웹페이지 파일
- `docs/evidence/` : 명령어 실행 결과와 브라우저 접속 결과를 저장한 증빙 폴더

---

## 4. 수행 내용

### 4-1. 웹페이지 작성
`app/index.html` 파일에 간단한 HTML 페이지를 작성하였다.

출력 문구:
- Hello Docker!
- 컨테이너에서 웹페이지가 정상 실행 중입니다.

### 4-2. Dockerfile 작성
정적 웹페이지를 Nginx 웹서버로 서비스하기 위해 다음과 같이 Dockerfile을 작성하였다.

```dockerfile
FROM nginx:alpine
COPY app/index.html /usr/share/nginx/html/index.html
EXPOSE 80
```

### 4-3. Dockerfile 설명
- `FROM nginx:alpine`  
  Nginx가 포함된 경량 Alpine 이미지를 기반으로 사용한다.
- `COPY app/index.html /usr/share/nginx/html/index.html`  
  로컬의 HTML 파일을 컨테이너 내부 Nginx 기본 웹 경로로 복사한다.
- `EXPOSE 80`  
  컨테이너 내부 웹서비스 포트가 80번임을 명시한다.

---

## 5. Docker 실행 과정

### 5-1. hello-world 실행
Docker가 정상적으로 동작하는지 확인하기 위해 `hello-world` 이미지를 실행하였다.

```bash
docker run hello-world
```

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 5-2. 이미지 목록 확인
```bash
docker images
```

```text
IMAGE                        ID             DISK USAGE   CONTENT SIZE   EXTRA
dev-workstation-web:latest   2afe702f7e8d       93.7MB         26.4MB
hello-world:latest           cf1b20a0c89a       25.9kB         9.49kB    U
my-web:latest                863985d35521       93.7MB         26.4MB    U
```

### 5-3. 컨테이너 목록 확인
```bash
docker ps -a
```

```text
CONTAINER ID   IMAGE         COMMAND                   CREATED          STATUS                     PORTS                                     NAMES
0c9101795027   hello-world   "/hello"                  2 minutes ago    Exited (0) 2 minutes ago                                             practical_kowalevski
7770ba4228fa   my-web        "/docker-entrypoint.…"   48 minutes ago   Up 48 minutes              0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-container
e62dd05acb1c   hello-world   "/hello"                  3 hours ago      Exited (0) 3 hours ago                                               jovial_kapitsa
```

### 5-4. Docker 이미지 빌드
다음 명령어를 사용하여 이미지를 빌드하였다.

```bash
docker build -t my-web .
```

```text
=> [1/2] FROM nginx:alpine
=> [2/2] COPY app/index.html /usr/share/nginx/html/index.html
=> exporting to image
=> => naming to docker.io/library/my-web
```

### 5-5. Docker 컨테이너 실행
다음 명령어를 사용하여 컨테이너를 실행하였다.

```bash
docker run -d -p 8080:80 --name my-web-container my-web
```

```text
7770ba4228fa
```

### 5-6. 실행 확인
브라우저에서 아래 주소로 접속하여 웹페이지가 정상적으로 출력되는 것을 확인하였다.

```text
http://localhost:8080
```

---

## 6. 실행 결과

### 6-1. 컨테이너 실행 상태 확인
다음 명령어로 실행 중인 컨테이너를 확인하였다.

```bash
docker ps
```

```text
CONTAINER ID   IMAGE     COMMAND                   CREATED             STATUS             PORTS                                     NAMES
7770ba4228fa   my-web    "/docker-entrypoint.…"   About an hour ago   Up About an hour   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web-container
```

확인 결과:
- 컨테이너 이름: `my-web-container`
- 이미지 이름: `my-web`
- 포트 매핑: `0.0.0.0:8080->80/tcp`
- 상태: `Up`

### 6-2. 브라우저 접속 결과
브라우저에서 `http://localhost:8080`에 접속한 결과, 다음 문구가 정상적으로 출력되었다.

```text
Hello Docker!
컨테이너에서 웹페이지가 정상 실행 중입니다.
```

---

## 7. 포트 매핑 설명
컨테이너 실행 명령어의 `-p 8080:80` 옵션은 **호스트의 8080번 포트**를 **컨테이너 내부의 80번 포트**와 연결한다는 의미이다.  
즉, 사용자는 `http://localhost:8080`으로 접속하지만 실제 요청은 컨테이너 내부 Nginx의 80번 포트로 전달된다.

### 7-1. 포트 포워딩이 필요한 이유
컨테이너 내부 포트는 외부에서 직접 접근할 수 없기 때문에, 브라우저에서 접속하려면 호스트 포트와 컨테이너 포트를 연결해야 한다.

### 7-2. 네트워크 네임스페이스 설명
Docker 컨테이너는 서로 격리된 네트워크 공간을 가지며, 이를 네트워크 네임스페이스라고 한다.  
따라서 컨테이너 내부의 80번 포트는 호스트의 포트와 독립적으로 동작하고, 외부에서 접근하려면 `-p` 옵션이 필요하다.

---

## 8. 이미지와 컨테이너의 차이

### 8-1. 이미지(Image)
이미지는 애플리케이션 실행에 필요한 파일과 설정을 포함한 템플릿이다.

### 8-2. 컨테이너(Container)
컨테이너는 이미지를 실제로 실행한 결과물이며, 동작 중인 실행 환경이다.

### 8-3. 빌드와 실행의 차이
- `docker build` : Dockerfile을 기준으로 이미지를 생성하는 단계
- `docker run` : 생성된 이미지를 기반으로 컨테이너를 실행하는 단계

즉, 이미지는 실행을 위한 설계도이고, 컨테이너는 실제 실행 중인 인스턴스이다.

---

## 9. 볼륨 마운트와 데이터 영속성

컨테이너는 삭제되면 내부 변경 내용이 사라질 수 있으므로, 데이터를 유지하려면 볼륨 또는 바인드 마운트를 사용할 수 있다.

### 9-1. 볼륨(바인드 마운트) 실행 예시
다음 명령어로 호스트의 `app` 폴더를 컨테이너 내부 웹 경로에 연결할 수 있다.

```bash
docker run -d -p 8081:80 --name volume-test -v %cd%\app:/usr/share/nginx/html nginx:alpine
```

또는 실제 수행한 명령어를 아래에 기록하였다.

```text
docker run -d -p 8081:80 --name volume-test -v %cd%\app:/usr/share/nginx/html nginx:alpine
```

### 9-2. 데이터 영속성 설명
호스트 폴더를 컨테이너에 마운트하면, 컨테이너를 삭제하더라도 호스트에 있는 파일은 그대로 유지된다.  
즉, 컨테이너는 제거되어도 `app/index.html` 같은 원본 파일은 삭제되지 않는다.

### 9-3. 확인 결과
```text
volume-test 컨테이너를 삭제한 후에도 호스트의 app 폴더와 그 안의 index.html 파일은 그대로 유지되었다. 즉, 컨테이너는 삭제되었지만 호스트에 저장된 데이터는 사라지지 않아 데이터 영속성이 확인되었다.
```

---

## 10. chmod 및 ls -l 확인

> Windows CMD에서는 `chmod`, `ls -l` 명령이 기본적으로 제공되지 않을 수 있으므로, Git Bash / WSL / Linux 컨테이너 내부에서 확인할 수 있다.

### 10-1. 실행 예시
```bash
chmod 644 app/index.html
ls -l app/index.html
```

```text
[실행 결과 붙여넣기]
```

### 10-2. 설명
- `chmod` : 파일 권한을 변경하는 명령어
- `ls -l` : 파일 권한, 크기, 소유자 등의 상세 정보를 확인하는 명령어

---

## 11. Git 설정 및 원격 저장소 확인

### 11-1. Git 사용자 정보 확인
```bash
git config --global user.name
git config --global user.email
```

```text
[실행 결과 붙여넣기]
```

### 11-2. 원격 저장소 확인
```bash
git remote -v
```

```text
[실행 결과 붙여넣기]
```

### 11-3. 설명
- `git config` : 사용자 이름과 이메일 설정을 확인하는 명령어
- `git remote -v` : 연결된 원격 저장소 주소를 확인하는 명령어

---

## 12. Docker 동작 원리
Docker는 애플리케이션과 실행에 필요한 환경을 하나의 이미지로 패키징하고, 이를 컨테이너 형태로 실행할 수 있게 해준다.  
컨테이너는 가볍고 빠르게 실행되며, 동일한 이미지를 사용하면 어느 환경에서도 비슷한 실행 결과를 얻을 수 있다.

이 프로젝트의 동작 과정은 다음과 같다.

1. `app/index.html` 파일을 작성한다.
2. `Dockerfile`을 작성한다.
3. `docker build -t my-web .` 명령어로 이미지를 빌드한다.
4. `docker run -d -p 8080:80 --name my-web-container my-web` 명령어로 컨테이너를 실행한다.
5. `docker ps` 명령어로 실행 상태를 확인한다.
6. 브라우저에서 `localhost:8080`으로 접속하여 결과를 확인한다.

---

## 13. 문제 해결 과정

### 문제 1. 컨테이너 이름 중복
`my-web-container`라는 이름이 이미 존재하여 새 컨테이너 생성이 되지 않는 문제가 있었다.

해결 방법:
```bash
docker rm -f my-web-container
docker run -d -p 8080:80 --name my-web-container my-web
```

### 문제 2. 실행 여부 확인
컨테이너가 정상적으로 실행되었는지 확인하기 위해 다음 명령어를 사용하였다.

```bash
docker ps
```

실행 결과에서 `Up` 상태와 `0.0.0.0:8080->80/tcp` 포트 매핑을 확인하여 정상 실행을 확인하였다.

### 문제 3. 포트 충돌 가능성
이미 다른 프로그램이 8080 포트를 사용 중이면 컨테이너 실행에 문제가 발생할 수 있다.

해결 방법:
- 기존 프로그램을 종료하거나
- 다른 포트로 변경하여 실행한다.

예시:
```bash
docker run -d -p 8081:80 --name my-web-container my-web
```

---

## 14. 증빙 자료

### 14-1. Docker 관련 증빙
- `docker version`
- `docker info`
- `docker run hello-world`
- `docker images`
- `docker ps -a`
- `docker build -t my-web .`
- `docker run -d -p 8080:80 --name my-web-container my-web`
- `docker ps`
- `localhost:8080` 접속 결과

### 14-2. Git 관련 증빙
- `git --version`
- `git config --global user.name`
- `git config --global user.email`
- `git remote -v`

### 14-3. 권한 및 볼륨 관련 증빙
- `chmod` 실행 결과
- `ls -l` 실행 결과
- 볼륨 마운트 실행 결과
- 컨테이너 삭제 후 파일 유지 확인 결과

### 14-4. 첨부 이미지
![docker-version](docs/evidence/docker-version.png)
![docker-info1](docs/evidence/docker-info1.png)
![docker-info2](docs/evidence/docker-info2.png)
![docker-images](docs/evidence/docker-images.png)
![docker-ps-a](docs/evidence/docker-ps-a.png)
![docker-hello-world](docs/evidence/docker-hello-world.png)
![docker-build](docs/evidence/docker-build.png)
![docker-run-web-test](docs/evidence/docker-run-web-test.png)
![localhost-8080](docs/evidence/localhost-8080.png)
![git-version](docs/evidence/git-version.png)
![git-config](docs/evidence/git-config.png)
![git-remote-v](docs/evidence/git-remote-v.png)
![chmod-ls-l](docs/evidence/chmod-ls-l.png)
![volume-run](docs/evidence/volume-run.png)
![volume-after-delete](docs/evidence/volume-after-delete.png)

---

## 15. 결론
이번 프로젝트를 통해 Docker 이미지 빌드, 컨테이너 실행, 포트 매핑, 브라우저 접속 확인까지 전체 과정을 직접 실습할 수 있었다.  
또한 이미지와 컨테이너의 차이, 볼륨을 통한 데이터 영속성, Git 설정 확인, 네트워크 네임스페이스와 포트 매핑 개념도 함께 정리할 수 있었다.  
Docker를 사용하면 동일한 실행 환경을 일관되게 구성할 수 있고, 정적 웹페이지도 손쉽게 컨테이너 기반으로 배포할 수 있음을 확인하였다.

---

## 16. 실행 명령어 정리

### Docker 버전 확인
```bash
docker version
```

### Docker 정보 확인
```bash
docker info
```

### hello-world 실행
```bash
docker run hello-world
```

### 이미지 목록 확인
```bash
docker images
```

### 컨테이너 목록 확인
```bash
docker ps -a
```

### 이미지 빌드
```bash
docker build -t my-web .
```

### 컨테이너 실행
```bash
docker run -d -p 8080:80 --name my-web-container my-web
```

### 실행 중인 컨테이너 확인
```bash
docker ps
```

### 컨테이너 중지
```bash
docker stop my-web-container
```

### 컨테이너 삭제
```bash
docker rm -f my-web-container
```

### Git 사용자 정보 확인
```bash
git config --global user.name
git config --global user.email
```

### 원격 저장소 확인
```bash
git remote -v
```