# Level 7 → 8

## 1. 목표 (Objective)
data.txt 내부 bandit8의 비밀번호 얻기
### 조건
millionth 옆에 존재

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `grep`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 파일 확인
2. `cat data.txt` 실행 → 많은 양의 string 출력
3. `cat data.txt | grep "millionth"` 실행

## 4. 원리 (Theory)
- `|`
  - 앞 명령어의 출력(결과물)을 뒤 명령어의 입력으로 바로 연결해 주는 통로
- `grep "millionth"`
  - millionth가 들어간 줄을 꺼내오라는 의미

---

# Level 8 → 9

## 1. 목표 (Objective)
data.txt 내부 bandit9의 비밀번호 얻기
### 조건
- 한 번만 등장하는 유일한 텍스트

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `sort`, `uniq`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. `sort -u data.txt` 실행 → 많은 양의 문자열 출력됨
3. `sort data.txt | uniq -c` 실행 → 횟수가 1인 줄이 비밀번호 후보

## 4. 원리 (Theory)
- `sort`
  - 파일 내부 내용을 정렬하는 명령어
  - `-u`: 유니크 옵션으로 하나씩만 남기기
- `uniq`
  - 하나씩만 남기기
  - 이웃한 줄만 비교하므로 `sort`로 먼저 정렬해야 함
  - `-c`: 각 줄의 등장 횟수를 앞에 표시
  - 단 한 번만 나오는 것만 찾기 위해서는 `-u` 옵션을 사용하는 방법도 있음.

---

# Level 9 → 10

## 1. 목표 (Objective)
data.txt 내부 bandit10의 비밀번호 얻기
### 조건
- `=` 여러 개가 존재

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `grep`, `strings`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. `cat data.txt | grep "="` 실행 → `grep: (standard input): binary file matches`
3. `cat data.txt | grep -a "="` 실행 → 보기 어려움
4. `strings data.txt | grep "="` 실행

## 4. 원리 (Theory)
- `grep`
  - `-a` : 바이너리 파일을 일반 텍스트 취급해 검색을 강제하는 옵션
- `strings`
  - 바이너리 파일을 그대로 내보내는 게 아닌 텍스트만 골라내서 출력하는 명령어

---
[← 목차로](./README.md)