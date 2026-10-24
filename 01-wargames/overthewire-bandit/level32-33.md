# Level 32 → 33

## 1. 목표 (Objective)
bandit에 들어가 bandit33의 비밀번호 얻기
### 조건
- escape
- Good luck

## 2. 사용 명령어 (Commands)
`ssh`

## 3. 풀이 과정 (Procedure)
1. `ssh`로 접속 → `WELCOME TO THE UPPERCASE SHELL`
2. `ls -al` 실행 → `sh: 1: LS: Permission denied`
    - 모든 글자가 대문자로 치환된다.
3. `$0` 실행 → `$` 명령어 입력창이 변환됨
4. `whoami` 실행 → `bandit33` 반환
5. `cat /etc/bandit_pass/bandit33` 실행


## 4. 원리 (Theory)
- `$0`
  - 현재 실행 중인 쉘의 이름이나 스크립트 파일명을 나타내는 특수 변수
  - 첫 error 메세지를 보면 `sh: 1` 이부분을 확인하면 다음과 같은 순서로 작동된다고 추론 가능.
    1. Uppercase
    2. sh
  - 따라서 uppercase는 알파벳만 대문자로 바꾸기 때문에 `$0`은 무시하고 넘겨 sh를 실행

---
[← 목차로](./README.md)