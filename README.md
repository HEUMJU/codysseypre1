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
```

---

## ✅ 지금 바로 채울 수 있는 것 (자료 확보됨)

| 섹션 | 자료 | 상태 |
|------|------|------|
| 2. 권한 | 4-3.txt | ✅ 바로 가능 (이미 받음) |
| 3~5 | 4-4, 4-5 | ✅ 파일 있음 |
| 6. Dockerfile | 4-7 + Dockerfile | ✅ 파일 있음 |
| 7. 포트매핑 | PNG | ✅ 확보 |
| 9. 볼륨 | 4-9 | ✅ 파일 있음 |
| 10. Git | 4-10 | ✅ 파일 있음 |
| 11. 트러블슈팅 | 2건 | ✅ 확보 |

---

## ⚠️ 추가 실습이 필요할 수 있는 것

**1. 터미널 기본 조작 전체 (섹션 1) — 우선 확인!**
- 현재 `4-3.txt`엔 `mkdir/touch/cd/ls`만 있어요.
- `pwd`, `cp`, `mv`, `rm`, `cat`, `ls -a` 로그가 **다른 파일에 있는지** 먼저 확인:
  ```bash
  grep -l "pwd\|cp \|mv \|rm " work/*.txt
  ```
- 없다면 아래 5줄만 실행해서 `work/4-2.txt`로 저장하면 끝:
  ```bash
  pwd
  ls -a
  cp file.txt copy.txt
  mv copy.txt renamed.txt
  rm renamed.txt
  cat file.txt
  ```

**2. 바인드 마운트 (섹션 8) — 중요!**
- `4-9.txt`는 **볼륨(volume)**이라 **바인드 마운트(-v 호스트경로:컨테이너경로)**는 별도일 수 있어요.
- 확인:
  ```bash
  grep -l "bind\|mount\|-v " work/*.txt
  ```
- 없으면 추가 실습 필요 (호스트 파일 수정 → 컨테이너 즉시 반영 확인).

**3. 자잘한 마무리**
- `<GitHub URL>` 실제 주소 기입
- PDF(2.8MB) `.gitignore` 처리 여부 결정

---

## 다음 단계 제안

먼저 이 두 명령만 돌려서 **섹션 1, 8 공백 여부**를 확정하죠:

```bash
grep -l "pwd\|cp \|mv " work/*.txt
grep -l "bind\|mount\|-v " work/*.txt
```

결과 보내주시면 **추가 실습이 진짜 필요한지** 바로 판단해드릴게요. 자료가 이미 다 있으면 README 채우기만 하면 돼요! 😊