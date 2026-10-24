# Level 18 → 19

## 1. 목표 (Objective)
홈 디렉토리에 존재하는 readme를 통해 bandit19 비밀번호 얻기
### 조건
- 접속 시 바로 로그아웃 됨

## 2. 사용 명령어 (Commands)
`ssh`

## 3. 풀이 과정 (Procedure)
1. `ssh`로 접속 → `Byebye !` 반환 후 연결 끊김
2. 로컬에서 `ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme` 실행

## 4. 원리 (Theory)
- `ssh`
  - `ssh {사용자}@{호스트} {명령어}` 형태로 입력하는 구조
  - 클라이언트가 SSH 연결 및 인증이 완료되면, 서버에 대화형 셸을 요청하는 대신 특정 명령어 실행을 요청하는 메세지를 보냄
  - 기본 셸이 아닌 비대화형 모드로 실행 후 `{명령어}`를 실행하고 클라이언트 터미널로 보낸 후 SSH 연결을 자동으로 종료

---
[← 목차로](./README.md)