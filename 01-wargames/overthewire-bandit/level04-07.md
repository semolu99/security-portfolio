# Level 4 → 5

## 1. 목표 (Objective)
inhere 디렉토리의 여러 파일들 중 유일하게 읽을 수 있는 파일을 찾아 bandit5의 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `file`

## 3. 풀이 과정 (Procedure)
1. `ls` 명령어로 홈 디렉토리 파일 확인
2. `ls -al ./inhere` 명령어로 inhere 내부 파일 확인
3. `file ./inhere/*` 명령어로 파일의 내부 형식 분석
4. `cat inhere/{읽을 수 있는 파일}` 실행

## 4. 원리 (Theory)
- `file`
  - 파일의 종류(type)를 확인하는 명령어
  - `*` 와일드카드를 사용해 한 번에 전부 실행시키는 게 가능

---

# Level 5 → 6

## 1. 목표 (Objective)
inhere 디렉토리 내부 어딘가 숨은 파일을 찾아 bandit6의 비밀번호 얻기
### 조건
- 사람이 읽을 수 있음
- 1033 바이트 크기
- 실행 불가

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `find`

## 3. 풀이 과정 (Procedure)
1. `ls -al ./inhere` 명령어로 inhere 내부 파일 확인
2. `find ./inhere/ -type f -size 1033c` 명령어로 파일 찾기
3. `cat {찾은 파일 경로}` 실행

## 4. 원리 (Theory)
- `find`
  - 파일을 찾는 명령어
  - `-type f` : 디렉토리가 아닌 파일만 탐색하는 옵션
  - `-size 1033c` : 1033바이트만 지정하는 옵션 / `c`는 바이트 옵션

---

# Level 6 → 7

## 1. 목표 (Objective)
어딘가 숨은 파일을 찾아 bandit7의 비밀번호 얻기
### 조건
- 소유자: bandit7
- 소유 그룹: bandit6
- 33바이트

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `find`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인 → 아무것도 없음
2. `find / -user bandit7 -group bandit6 -size 33c` 명령어로 파일 찾기
3. `cat {찾은 파일 경로}` 실행

## 4. 원리 (Theory)
- `find`
  - 파일을 찾는 명령어
  - `-user {username}` : 소유자를 지정하는 옵션
  - `-group {groupname}` : 소유그룹을 지정하는 옵션

---
[← 목차로](./README.md)