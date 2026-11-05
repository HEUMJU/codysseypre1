# AI SW 개발 워크스페이스 구축 (Docker + Git)

## 개요
- 목적: Git/GitHub 연동 + Docker 기반 워크스페이스 구축 및 검증
- 환경: Windows / Git Bash (UCRT64) / Docker Desktop
- 저장소: <GitHub 저장소 URL>

## 목차
1. 터미널 기본 조작
2. 권한 실습 (전/후 비교)
3. Docker 설치 및 점검
4. Docker 이미지·컨테이너 운영
5. 컨테이너 실행/종료 (attach vs exec)
6. Dockerfile 커스텀 이미지 빌드
7. 포트 매핑 및 브라우저 접속
8. 바인드 마운트 (호스트↔컨테이너)
9. 볼륨 영속성 검증
10. Git / GitHub 연동
11. 트러블슈팅
12. 체크리스트

---

## 1. 터미널 기본 조작
> work/4-3.txt (일부)
```bash
(pwd / ls -a / cd / mkdir / touch / cp / mv / rm / cat 로그)
```

## 2. 권한 실습 (전/후 비교)
> work/4-3.txt
```bash
(chmod 600 file.txt / chmod 700 mydir 전후 ls -l)
```

## 3. Docker 설치 및 점검
> work/4-4.txt
```bash
(docker --version / docker info)
```

## 4. 이미지·컨테이너 운영
> work/4-5.txt, 4-5-2.txt
```bash
(docker images / ps -a / logs / stats 등)
```

## 5. 컨테이너 실행/종료 (attach vs exec)
> work/4-6-1~3.txt, exit exec.txt
```bash
(hello-world, ubuntu 실행 / exit 후 상태 관찰)
```

## 6. Dockerfile 커스텀 이미지 빌드
> work/4-7.txt + Dockerfile + index.html
```dockerfile
(Dockerfile 내용)
```
```bash
(docker build / run 로그)
```

## 7. 포트 매핑 및 브라우저 접속
> work/4-8 ....png
![브라우저 접속](work/4-8%20포트매핑%20및%20접속%20증거%20브라우저%20접속%20화면.png)
```bash
(docker run -p 8080:80 ... 명령)
```

## 8. 바인드 마운트 (호스트↔컨테이너)
> ⚠️ 로그 확인 필요
```bash
(docker run -v 호스트경로:컨테이너경로 / 호스트 수정 → 컨테이너 반영 확인)
```

## 9. 볼륨 영속성 검증
> work/4-9.txt
```bash
(volume create / 연결 / 컨테이너 삭제 후 데이터 유지 확인)
```

## 10. Git / GitHub 연동
> work/4-10.txt
```bash
(git init / remote add / push / git config --list)
```

## 11. 트러블슈팅
> work/트러블슈팅#1.txt, #2.txt

### 문제 1
- 증상 / 원인 / 해결

### 문제 2
- 증상 / 원인 / 해결

## 12. 체크리스트
> work/체크리스트.txt