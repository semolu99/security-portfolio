# Level 21 → 22

## 1. 목표 (Objective)
cron을 통해 bandit22 비밀번호 얻기
### 조건
`/etc/cron.d/`에 존재

## 2. 사용 명령어 (Commands)
`ls`, `cat`

## 3. 풀이 과정 (Procedure)
1. `ls -la /etc/cron.d` 실행
2. `cat /etc/cron.d/cronjob_bandit22` 실행 → `@reboot bandit22 {경로} &> /dev/null`
3. `ls -al {경로/파일.sh}` 실행 → `-rwxr-x--- 1 bandit22 bandit21`
4. `cat {경로/파일.sh}` 실행
    - 파일 권한 수정 및 비밀번호 읽은 값을 저장한 경로 정보 확인
5. `cat {경로}` 실행

## 4. 원리 (Theory)
- cron
  - 리눅스 및 유닉스 계열 운영체제에서 특정 시간에 특정 명령어나 스크립트를 자동으로 실행해 주는 예약 작업 스케줄러
  - 주로 정기적인 백업, 시스템 점검 및 디스크 정리, 주기적인 스크립트 실행이 필요할 때 사용됨

---

# Level 22 → 23

## 1. 목표 (Objective)
cron을 통해 bandit23 비밀번호 얻기
### 조건
- `/etc/cron.d/`에 존재

## 2. 사용 명령어 (Commands)
`cat`, `ls`

## 3. 풀이 과정 (Procedure)
1. `ls -la /etc/cron.d` 실행
2. `cat /etc/cron.d/cronjob_bandit23` 실행 → `@reboot bandit23 {경로} &> /dev/null`
3. `ls -al {경로/파일.sh}` 실행 → `-rwxr-x--- 1 bandit23 bandit22`
4. `cat {경로/파일.sh}` 실행
    - `mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)` 중요 구문 해당 부분을 변경하기로 판단
5. 스크립트 복붙 후 변경하여 실행 → `mytarget` 획득
6. `cat {/tmp/mytarget}` 실행


## 4. 원리 (Theory)
- `md5sum`
  - 특정파일의 MD5 해시값을 계산하여 파일이 손상되거나 변조되지 않았는지 무결성을 검증하는 명령어
  - MD5 해시값을 생성해서 예측할 수 없는 임시 파일의 이름을 계산하는 원리로 해결

---

# Level 23 → 24

## 1. 목표 (Objective)
cron을 통해 bandit24 비밀번호 얻기
### 조건
- `/etc/cron.d/`에 존재
- 쉘 스크립트 작성 필요

## 2. 사용 명령어 (Commands)
`cat`, `ls`, `mktemp`, `vi`, `chmod`, `cp`

## 3. 풀이 과정 (Procedure)
1. `ls -la /etc/cron.d` 실행
2. `cat /etc/cron.d/cronjob_bandit24` 실행 → `@reboot bandit24 {경로} &> /dev/null`
3. `ls -al {경로/파일.sh}` 실행 → `-rwxr-x--- 1 bandit24 bandit23`
4. `cat {경로/파일.sh}` 실행
    - `echo "Executing and deleting all scripts in /var/spool/$myname/foo:"` 중요 구문
5. `mktemp -d` 실행 → `cd {경로}` 실행
6. `vi {파일명.sh}` 실행 → 다음 내용 작성
    ```shell
    #!/bin/bash
    cat /etc/bandit_pass/bandit24 > /tmp/경로/result.txt
    ```
7. `chmod 777 {현재 디렉토리 경로}` 실행
8. `chmod 777 {파일명.sh}` 실행
9. `cp {파일명.sh} {스크립트에 적힌 경로/파일명.sh}` 실행
10. 대기 후 `ls -al` → `result.txt` 생성되기 까지 대기
11. `cat result.txt` 실행


## 4. 원리 (Theory)
- 쉘 스크립트
  - 작성 된 것을 읽어보면 `/var/spool/$myname/foo` 해당 경로에 있는 모든 스크립트를 실행 시키고 삭제하는 것으로 확인 됨
  - 따라서 쉘 스크립트를 작성하고, 경로에 넣어두면 스케줄러로 작동된다고 판단
- `vi`
  - 리눅스 표준 텍스트 편집기
  - 단축키와 명령어 요약은 아래와 같음

| 분류 | 단축키 / 명령어 | 기능 설명 |
| :--- | :--- | :--- |
| 모드 전환 | `i` | 글을 작성할 수 있는 입력 모드로 진입 |
| 모드 전환 | `Esc` | 글 쓰기를 마치고 명령 모드로 복귀 |
| 편집 (명령 모드) | `dd` | 현재 커서가 있는 한 줄 삭제 |
| 편집 (명령 모드) | `yy` | 현재 커서가 있는 한 줄 복사 |
| 편집 (명령 모드) | `p` | 복사하거나 잘라낸 내용을 커서 아래에 붙여넣기 |
| 종료 (콜론 입력) | `:w` | 변경된 내용 저장 |
| 종료 (콜론 입력) | `:q` | 편집기 종료 |
| 종료 (콜론 입력) | `:wq` | 저장 후 종료 |
| 종료 (콜론 입력) | `:q!` | 저장하지 않고 강제 종료 |

---
[← 목차로](./README.md)