# Troubleshooting

## 트러블슈팅 #1. PowerShell 종료 후 Docker 컨테이너 재접속 및 이름 중복 오류

### 1. 문제 상황

PowerShell이 느려지는 현상이 발생하여 터미널을 종료한 후 다시 실행했다.

기존에 `mybox`라는 이름으로 생성한 Ubuntu 컨테이너는 `exit` 명령으로 종료하지 않았기 때문에 PowerShell을 종료한 뒤에도 실행 상태를 유지하고 있었다.

새로운 PowerShell에서 동일한 이름으로 Ubuntu 컨테이너를 다시 생성하려고 하자 컨테이너 이름 중복 오류가 발생했다.

### 2. 오류 발생

실행 명령:
```powershell
docker run -it --name mybox ubuntu
```

오류 메시지:
```text
docker: Error response from daemon: Conflict.
The container name "/mybox" is already in use by container
"9e4656b1802dca03aa8f9e8aa9d7c7a6cd37d8f3a6031f45e06de0b4e5ff2c91".
You have to remove (or rename) that container to be able to reuse that name.
```

### 3. 원인 분석

- 기존 `mybox` 컨테이너가 종료되지 않고 실행 중인 상태였음.
- PowerShell을 종료해도 Docker Desktop의 Docker 데몬과 컨테이너는 별도로 동작할 수 있음.
- Docker는 이미 사용 중인 컨테이너 이름을 중복해서 사용할 수 없으므로 이름 충돌 오류가 발생함.

### 4. 해결 과정

**① 새로운 Ubuntu 컨테이너 실행**

기존 `mybox`를 삭제하지 않고 별도의 이름을 자동으로 부여받는 컨테이너를 실행했다.

```powershell
docker run -it ubuntu
```

실행 결과:
```text
root@9e723c5a425e:/# exit
exit
```

새로 생성된 컨테이너의 자동 이름은 `magical_bhaskara`였다.

**② 컨테이너 상태 확인**

```powershell
docker ps -a
```

실행 결과:
```text
CONTAINER ID   IMAGE         COMMAND       CREATED              STATUS                      PORTS     NAMES
9e723c5a425e   ubuntu        "/bin/bash"   About a minute ago   Exited (0) 36 seconds ago             magical_bhaskara
9e4656b1802d   ubuntu        "/bin/bash"   9 minutes ago        Up 9 minutes                          mybox
78aca6db2842   hello-world   "/hello"      44 minutes ago       Exited (0) 44 minutes ago             nice_morse
```

확인 결과:
- `magical_bhaskara`: `Exited (0)` 상태
- `mybox`: `Up 9 minutes` 상태
- 기존 컨테이너가 정상적으로 실행 중인 것을 확인함.

**③ 기존 mybox 컨테이너에 재접속**

```powershell
docker exec -it mybox bash
```

실행 결과:
```text
root@9e4656b1802d:/#
```

기존에 실행 중이던 `mybox` 컨테이너 내부 Bash에 정상적으로 재접속했다.

### 5. 해결 결과 및 학습 내용

- PowerShell 종료와 Docker 컨테이너 종료는 서로 다른 개념임을 확인했다.
- 컨테이너가 실행 중이면 동일한 이름으로 새 컨테이너를 생성할 수 없다.
- `docker ps -a` 명령으로 실행 및 종료 상태를 확인할 수 있다.
- 실행 중인 컨테이너에 다시 접속할 때는 `docker exec -it 컨테이너명 bash` 명령을 사용할 수 있다.
- 기존 컨테이너를 유지하면서 작업을 이어갈 수 있음을 확인했다.

### 6. 핵심 명령어 정리

| 명령어 | 기능 |
|---|---|
| `docker run -it ubuntu` | Ubuntu 컨테이너 생성 및 실행 |
| `docker ps -a` | 전체 컨테이너 상태 확인 |
| `docker exec -it mybox bash` | 실행 중인 컨테이너에 새 Bash 세션으로 접속 |
| `docker stop mybox` | 실행 중인 컨테이너 정지 |
| `docker rm mybox` | 정지된 컨테이너 삭제 |

---

## 트러블슈팅 #2. Dockerfile 저장 누락으로 인한 이미지 빌드 실패

### 1. 문제 상황

NGINX 공식 이미지를 기반으로 개인 HTML 페이지를 실행하기 위해 `index.html`과 `Dockerfile`을 작성했다.

이후 Dockerfile을 이용해 커스텀 이미지를 생성하려고 `docker build -t my-web .` 명령을 실행했으나 빌드에 실패했다.

### 2. 오류 발생

실행 명령:
```powershell
docker build -t my-web .
```

오류 메시지:
```text
[+] Building 0.1s (1/1) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 31B
ERROR: failed to build: failed to solve:
the Dockerfile cannot be empty
```

### 3. 원인 분석

- `index.html`과 `Dockerfile`을 작성했지만 파일을 저장하지 않은 상태였음.
- Docker가 빌드 과정에서 정상적인 Dockerfile 내용을 읽지 못함.
- 그 결과 Dockerfile이 비어 있다는 오류가 발생함.

### 4. 해결 과정

**① Dockerfile 및 HTML 파일 저장**

작성한 `Dockerfile`과 `index.html` 파일을 저장했다.

**② 이미지 재빌드**

```powershell
docker build -t my-web .
```

실행 결과:
```text
[+] Building 11.7s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 111B
 => [internal] load metadata for docker.io/library/nginx:latest
 => [1/2] FROM docker.io/library/nginx:latest
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to docker.io/library/my-web:latest
 => => unpacking to docker.io/library/my-web:latest
```

결과:
- Dockerfile 정상 인식.
- NGINX 공식 이미지 기반 빌드 성공.
- `index.html`을 NGINX 웹 루트 경로에 복사.
- `my-web:latest` 커스텀 이미지 생성 성공.

**③ 컨테이너 실행**

```powershell
docker run -d -p 8080:80 --name my-web my-web
```

실행 결과:
```text
bbc360907963b869b08367b0eddecb0c7d0183a0544ad15b301707246ef1f7dd
```

**④ 실행 상태 확인**

```powershell
docker ps
```

실행 결과:
```text
CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS          PORTS                                     NAMES
bbc360907963   my-web    "/docker-entrypoint.…"   34 seconds ago   Up 33 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-web
```

- 컨테이너 상태 `Up` 확인.
- 호스트 8080 포트와 컨테이너 80 포트 연결 확인.

### 5. 해결 결과 및 학습 내용

- Docker 빌드 전에 Dockerfile과 관련 파일이 정상적으로 저장되어 있는지 확인해야 한다.
- Docker는 현재 디렉터리(`.`)를 빌드 컨텍스트로 사용하므로 파일 위치도 중요하다.
- Dockerfile을 정상적으로 저장한 후 이미지 빌드 및 컨테이너 실행에 성공했다.
- NGINX 기반 커스텀 웹 서버를 구성하고 포트 매핑까지 완료했다.

### 6. 핵심 명령어

| 명령어 | 기능 |
|---|---|
| `docker build -t my-web .` | Dockerfile을 이용해 커스텀 이미지 빌드 |
| `docker run -d -p 8080:80 --name my-web my-web` | 커스텀 이미지 컨테이너 실행 |
| `docker ps` | 실행 중인 컨테이너 확인 |

