# Level 10 → 11

## 1. 목표 (Objective)
data.txt 내부 bandit11의 비밀번호 얻기

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `base64`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. `cat data.txt` 실행
3. `base64 -d < data.txt` 실행

## 4. 원리 (Theory)
- `base64`
  - base64로 인코딩, 디코딩 관련 명령어
  - `-d` : 디코딩을 지정하는 옵션
- `<`
  - 파일 내용을 표준 입력으로 전달하는 리다이렉션 연산자

---

# Level 11 → 12

## 1. 목표 (Objective)
data.txt 내부 bandit12의 비밀번호 얻기
### 조건
- 13단계 회전됨

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `tr`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. `cat data.txt` 실행
3. `tr 'N-ZA-Mn-za-m' 'A-Za-z' < data.txt` 실행

## 4. 원리 (Theory)
- `tr`
  - 문자 치환, 문자 삭제 등 문자열 변환 관련 명령어
  - `tr {원본} {변경본}` : 원본을 변경본으로 변경하는 내용
    - A가 13번 돌아가 N이므로 이와 같이 변경
- ROT13
  - 알파벳을 13칸 회전시키는 시저 암호로, 한 번 더 적용하면 원문으로 복원됨

---

# Level 12 → 13

## 1. 목표 (Objective)
data.txt 내부 bandit13의 비밀번호 얻기
### 조건
- 반복적으로 압축된 파일의 헥사덤프

## 2. 사용 명령어 (Commands)
`ls`, `cat`, `xxd`, `file`, `gzip`, `mktemp`, `cp`, `mv`, `bzip2`, `tar`, `cd`

## 3. 풀이 과정 (Procedure)
1. `ls -al` 명령어로 홈 디렉토리 내부 파일 확인
2. `cat data.txt` 실행
3. 여러 번 압축을 풀어야 할 것으로 판단되어 `mktemp -d` 실행
4. `cp data.txt {생성된 공간}/data.txt`를 통해 복사
5. `cd {생성된 공간}` 명령어 사용으로 이동
6. `xxd -r data.txt > result.txt` 명령어로 헥사덤프로 복원 진행
7. `cat result.txt` 실행 → 읽을 수 없음
8. `file result.txt`를 통해 파일의 형식 확인 → `gzip compressed data`
9. `gzip -d result.txt` 실행 → `gzip: result.txt: unknown suffix -- ignored`
10. `mv result.txt result.gz` 실행
11. `gzip -d result.gz` 실행
12. `ls -al` 파일 확인
13. `file result` 실행 → `result: bzip2 compressed data, block size = 900k`
14. 이후 반복 작업으로 인한 하단 표로 대체

| file 결과 | 사용 명령어 |
|-----------|-------------|
| `gzip compressed data` | 확장자 `.gz`로 바꾼 뒤 `gzip -d {fileName}` |
| `bzip2 compressed data` | `bzip2 -d -c {fileName} > {result-fileName}` |
| `POSIX tar archive (GNU)` | `tar -xvf {fileName}` |

15. `file {fileName}` 실행 → `{fileName}: ASCII text`
16. `cat {fileName}` 실행


## 4. 원리 (Theory)
- `mktemp -d`
  - 홈 디렉토리를 사용할 수 없었기 때문에 `/tmp` 폴더 내에 임시 폴더를 생성해서 문제를 해결함
- `xxd`
  - 헥사 덤프 툴로 파일의 내용을 16진수와 ASCII 문자로 변환하여 보여주거나 반대로 역변환하는 명령어
  - `-r`: 16진수 텍스트를 다시 바이너리 파일로 복원하는 옵션
- `gzip`
  - GNU zip으로 단일 압축하는데 특화된 압축 표준 도구
  - `.gz` 확장자만 가능했기 때문에 에러가 발생했던 것
  - `-d`: 압축 해제
  - `-c`: 작업한 내용을 파일로 바꾸지 않고 바로 출력 (mv 없이도 가능)
- `bzip2`
  - `gzip`보다 압축률이 더 우수하지만, 압축/해제 속도가 좀 더 느린 압축 도구
  - `-d`: 압축 해제
  - `-c`: 작업한 내용을 파일로 바꾸지 않고 바로 출력
- `tar`
  - 압축 도구가 아니라 여러 개 파일과 디렉토리를 하나의 파일로 묶어주는 아카이브 도구
  - `-x`: 묶인 아카이브 풀기
  - `-v`: 과정을 화면에 출력
  - `-f`: 대상 아카이브 파일 지정 (옵션 맨 뒤에 필수 지정)
  - `-c`: 파일 묶기 (새 아카이브 생성)

---
[← 목차로](./README.md)