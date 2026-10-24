# Level 25 → 26

## 1. 목표 (Objective)
bandit26 로그인해 bandit26의 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`ssh`, `scp`, `grep`, `vi`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 실행 → `bandit26.sshkey` 존재
2. 로컬에서 `scp -P 2220 bandit25@bandit.labs.overthewire.org:bandit26.sshkey ./` 실행
3. 로컬에서 `ssh -i bandit26.sshkey -p 2220 bandit26@bandit.labs.overthewire.org` 실행
    - 바로 연결이 끊김
    - 사이트 확인 결과 로그인 쉘을 찾아볼 것으로 작성 되어 있음
    - bandit25로 다시 접속
4. `grep "bandit26" /etc/passwd` 실행
5. `cat {기본 쉘 경로}` 실행 → `exec more ~/text.txt` 중요정보
6. 화면을 작게 만들고 다시 bandit26 접속 → 화면 하단에 `--more--`가 나타나면 화면 크기 원복
7. `v`를 눌러 `vi`모드로 전환
8. `:set shell=/bin/bash` 실행
9. `:shell` 실행
10. `cat /etc/bandit_pass/bandit26` 실행

## 4. 원리 (Theory)
- `/etc/passwd`
  - 시스템에 등록된 모든 사용자 계정의 기본 정보를 저장하는 텍스트 파일
  - 모든 사용자가 읽을 수 있음
  - 총 7개 필드가 존재함
  - `사용자이름:비밀번호:사용자ID:그룹ID:사용자설명:홈디렉토리:기본셸`
- `more`
  - 페이지 단위로 끊어서 볼 수 있게 해주는 페이저 프로그램
  - `v`단축키: 현재 보고 있는 파일을 vi 편집기로 즉시 열기
- `vi`
  - 쉘 호출 기능이 존재
  - `:set shell=/bin/bash` 쉘을 띄울 때 어떤 프로그램을 기본으로 쓸지 설정하는 옵션

---
[← 목차로](./README.md)