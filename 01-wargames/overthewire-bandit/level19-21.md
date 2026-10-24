# Level 19 → 20

## 1. 목표 (Objective)
`/etc/bandit_pass`를 통해 bandit20 비밀번호 얻기
### 조건
- setuid 존재

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `id`, `whoami`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 실행
2. `./bandit20-do` 실행 → `Example: ./bandit20-do whoami`
3. `./bandit20-do whoami` 실행 → `bandit20`
4. `./bandit20-do id` 실행 → `uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)` 권한이 상승 됨이 확인됨
5. `./bandit20-do cat /etc/bandit_pass/bandit20` 실행

## 4. 원리 (Theory)
- setuid
  - 모든 사용자에게 상위 권한을 줄 수 없기 때문에, 특정 프로그램을 실행하는 동안에만 해당 프로그램 소유자의 권한으로 일시 상승시키는 시스템 설정
  - 따라서 현재 문제에서 나온 `bandit20-do`를 사용하면 일시적으로 bandit20의 권한으로 명령어 실행 가능

---

# Level 20 → 21

## 1. 목표 (Objective)
홈 디렉토리에 존재하는 setuid를 사용해 bandit21의 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`nc`, `ls`, `tmux`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 실행
2. `./suconnect` 실행 → `Usage: ./suconnect <portnumber>`
    - 추가적으로 포트를 열고 기다려야한다고 한다.
3. 하나의 화면에서 여러 터미널 작업을 위해 `tmux` 실행
4. 가상 터미널에서 `nc -l 7777` 실행 후 현재 비밀번호 입력
5. `ctrl` + `B` 누른 후 `D` 눌러 세션 나오기
6. `./suconnect 7777` 실행 → `Password matches, sending next password`
7. `tmux a` 실행해서 가상 터미널 진입 → 출력된 비밀번호 확인


## 4. 원리 (Theory)
- `tmux`
  - 하나의 터미널 창을 여러 개의 화면으로 분할하거나, SSH 연결이 끊어져도 백그라운드에서 터미널 세션을 유지하는 터미널 관리 도구
  - `Ctrl + B` → `D` (`tmux detach`): 세션 밖으로 나가기
  - `tmux a`: 가장 마지막에 활성화되었던 세션으로 복귀
- `nc`
  - `-l`: Listening 모드로 실행하는 옵션

---
[← 목차로](./README.md)