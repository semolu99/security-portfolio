# Level 26 → 27

## 1. 목표 (Objective)
bandit26 로그인해 bandit27의 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`ls`, `id`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 실행 → `bandit27-do` 존재
2. `./bandit27-do` 실행 → `Example: ./bandit27-do id`
3. `./bandit27-do id` 실행 → `uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)`
4. `./bandit27-do cat /etc/bandit_pass/bandit27` 실행

## 4. 원리 (Theory)
- setuid
  - 기존 setuid와 동일

---
[← 목차로](./README.md)