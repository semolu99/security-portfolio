# Level 13 → 14

## 1. 목표 (Objective)
`/etc/bandit_pass/bandit14`를 얻거나 홈의 ssh 키를 사용

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `scp`, `ssh`, `chmod`

## 3. 풀이 과정 (Procedure)
1. 접속 후 `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. 로컬 터미널에서 `scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private ./` 실행
3. 로컬 터미널에서 `ssh -i ./sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org` 실행
   - `Permissions 0640 for './sshkey.private' are too open.` 따라서 권한 조정
4. 로컬 터미널에서 `chmod 400 sshkey.private` 후 다시 접속
5. `cat /etc/bandit_pass/bandit14` 비밀번호 획득

## 4. 원리 (Theory)
- `scp`
  - SSH 프로토콜을 사용해서 네트워크를 통해 안전하게 파일을 복사하는 명령어
  - `-P`: 포트 번호를 설정하는 옵션
- `ssh`
  - `-i {파일 경로}`: RSA 등 인증에 사용할 개인키 파일을 지정하는 옵션
- `chmod`
  - 너무 많은 권한이 열려 있어, 위험할 수 있기 때문에 거부됨.
  - `400`: 앞자리부터 소유자, 소유그룹, 그 외 모든 사용자를 의미하며, 숫자 의미는 아래 표 참고

| 숫자 | 의미 |
|-----------|-------------|
| `0` | 권한 없음(`---`) |
| `1` | 실행(`--x`) |
| `2` | 쓰기(`-w-`) |
| `3` | 실행+쓰기(`-wx`) |
| `4` | 읽기(`r--`) |
| `5` | 읽기+실행(`r-x`) |
| `6` | 읽기+쓰기(`rw-`) |
| `7` | 모두(`rwx`) |

---

# Level 14 → 15

## 1. 목표 (Objective)
localhost 30000포트에 접속해 현재 비밀번호(bandit14)를 제출

## 2. 사용 명령어 (Commands)
`nc`

## 3. 풀이 과정 (Procedure)
1. `nc localhost 30000` 실행
2. `{비밀번호 입력}`

## 4. 원리 (Theory)
- `nc`
  - 네트워크 연결을 통해 데이터를 읽고 쓰는 유틸리티
  - 보안 점검, 포트 스캔, 파일 전송, 간이 서버 구축 등에도 사용됨
  - 암호화 없이 평문으로 통신함

---

# Level 15 → 16

## 1. 목표 (Objective)
localhost 30001포트에 접속해 현재 비밀번호(bandit15)를 제출
### 조건
- `SSL/TLS` 암호화 상태

## 2. 사용 명령어 (Commands)
`openssl`

## 3. 풀이 과정 (Procedure)
1. `openssl s_client -connect localhost:30001` 실행
2. `{비밀번호 입력}`

## 4. 원리 (Theory)
- `openssl`
  - `s_client`: 서버에 SSL/TLS로 접속해 인증서·핸드셰이크 정보를 보여 주는 클라이언트 도구
  - 보안 설정이 보고 싶지 않다면 `-quiet` 등 옵션을 추가할 수 있다.

---

# Level 16 → 17

## 1. 목표 (Objective)
localhost에 접속해 현재 비밀번호(bandit16)를 제출
### 조건
- 포트 번호는 31000~32000 사이

## 2. 사용 명령어 (Commands)
`nmap`, `openssl`

## 3. 풀이 과정 (Procedure)
1. `nmap -sV -p 31000-32000 localhost` 실행
2. `nmap` 결과는 다음 표와 같음

| PORT | SERVICE |
|-----------|-------------|
| `{port}/tcp` | echo |
| `{port}/tcp` | ssl/echo |
| `{port}/tcp` | echo |
| `{port}/tcp` | ssl/unknown |
| `{port}/tcp` | echo |

3. `openssl s_client localhost:{port}` 실행 후 비밀번호 입력 → `KEYUPDATE` 반환
4. `openssl s_client -quiet localhost:{port}` 실행 후 비밀번호 입력

## 4. 원리 (Theory)
- `nmap`
  - 네트워크 스캔 도구
  - `-sV`: 열린 포트에 대해 실제 그 포트에 특정한 패킷을 넣어 돌아오는 응답을 분석하는 옵션
  - `-p`: 포트 지정 옵션으로 `-`를 통해 범위 지정 가능
- `-quiet`
  - 반딧 사이트에 helpful note를 참고해서 옵션에 대한 내용을 찾아봄
  - Connected Commands를 무시할 수 있는 명령어로 더 있었지만 `-quiet`를 사용
  - [옵션관련 자료](https://docs.openssl.org/4.0/man1/openssl-s_client/#options)

---
[← 목차로](./README.md)