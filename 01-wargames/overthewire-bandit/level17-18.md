# Level 17 → 18

## 1. 목표 (Objective)
홈 디렉토리에 존재하는 passwords.old, passwords.new를 통해 bandit18 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`ls`, `diff`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 홈 디렉토리 파일 확인
2. `diff passwords.new passwords.old` 실행

## 4. 원리 (Theory)
- `diff`
  - 리눅스 혹은 유닉스 기반 시스템에서 두 개의 파일 또는 디렉토리를 줄 단위로 비교하여 차이점을 찾아주는 명령어
    - 먼저 입력된 파일이 우선 작성이 된다 new의 방향을 확인하면 얻을 수 있다.

---
[← 목차로](./README.md)