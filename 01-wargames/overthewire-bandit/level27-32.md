# Level 27 → 28

## 1. 목표 (Objective)
git을 사용해 bandit28 비밀번호 얻기
### 조건
- 로컬에서 진행

## 2. 사용 명령어 (Commands)
`git`

## 3. 풀이 과정 (Procedure)
풀이과정은 로컬에서만 진행 했습니다
1. `git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo` 실행
2. `ls -al` 실행
3. `ls -al ./repo` 실행
4. `cat ./repo/README` 실행

## 4. 원리 (Theory)
- `git`
  - 소스코드의 버전을 관리하는 명령어
  - `clone`: 원격 저장소(GitHub 등)에 저장된 소스코드를 로컬에 가져오는 옵션

---

# Level 28 → 29

## 1. 목표 (Objective)
git을 사용해 bandit29 비밀번호 얻기
### 조건
- 로컬에서 진행

## 2. 사용 명령어 (Commands)
`git`

## 3. 풀이 과정 (Procedure)
풀이과정은 로컬에서만 진행 했습니다
1. `git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo` 실행
2. `ls -al` 실행
3. `ls -al ./repo` 실행
4. `cat ./repo/README.md` 실행 → 비밀번호가 가려짐
5. `cd repo` 실행
6. `git log` 실행 → `fix info leak` 중요 정보 확인
7. `git show` 실행

## 4. 원리 (Theory)
- `git`
  - `log`: 프로젝트의 커밋 히스토리를 확인하는 명령어(최신부터 역순)
  - `show`: 특정 커밋 버전의 내용을 상세히 보는 명령어
    - `git show {target-commit-id}`: 커밋id는 7글자가 표준이자 관례
    - 미입력시 가장 최근 커밋 대상

---

# Level 29 → 30

## 1. 목표 (Objective)
git을 사용해 bandit30 비밀번호 얻기
### 조건
- 로컬에서 진행

## 2. 사용 명령어 (Commands)
`git`

## 3. 풀이 과정 (Procedure)
풀이과정은 로컬에서만 진행 했습니다
1. `git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo` 실행
2. `ls -al` 실행
3. `ls -al ./repo` 실행
4. `cat ./repo/README.md` 실행 → 비밀번호가 가려짐
5. `cd repo` 실행
6. `git log` 실행 → 특별한 기록이 없음
7. `git log --all` 실행 → `(origin/dev)` 발견
8. `git show {커밋ID}` 실행

## 4. 원리 (Theory)
- `git`
  - `--all`: 현재 브랜치의 로그 기록 이외에 모든 브랜치들의 기록 확인

---

# Level 30 → 31

## 1. 목표 (Objective)
git을 사용해 bandit31 비밀번호 얻기
### 조건
- 로컬에서 진행

## 2. 사용 명령어 (Commands)
`git`

## 3. 풀이 과정 (Procedure)
풀이과정은 로컬에서만 진행 했습니다
1. `git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo` 실행
2. `ls -al` 실행
3. `ls -al ./repo` 실행
4. `cat ./repo/README.md` 실행 → 특별한 내용이 없음
5. `cd repo` 실행
6. `git log` 실행 → 특별한 기록이 없음
7. `git log --all` 실행 → 특별한 기록이 없음
8. `git tag` 실행 → `secret` 발견
9. `git show secret` 실행

## 4. 원리 (Theory)
- `git`
  - `tag`: 깃의 참조 중 하나로, 특정 커밋의 해시값을 고정으로 가리키는 포인터

---

# Level 31 → 32

## 1. 목표 (Objective)
git을 사용해 bandit32 비밀번호 얻기
### 조건
- 로컬에서 진행

## 2. 사용 명령어 (Commands)
`git`

## 3. 풀이 과정 (Procedure)
풀이과정은 로컬에서만 진행 했습니다
1. `git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo` 실행
2. `ls -al` 실행
3. `ls -al ./repo` 실행
4. `cat ./repo/README.md` 실행 → 파일을 push하라는 내용
5. `cd repo` 실행
6. `vi key.txt` 실행
7. `git status` 실행 → `git add` 사용해봤지만 달라지는 내용이 없음
8. `cat .gitignore` 실행 → `*.txt` 확인
9. `vi .gitignore` 실행 → 내용 삭제
10. `git add .` 실행 → `git status` 실행 올라간것이 확인됨
11. `git commit -m "add key.txt"; git push` 실행

## 4. 원리 (Theory)
- `git`
  - `add`: 깃이 관리하는 파일에 추가하는 명령어
    - `.`: 모든 파일을 의미
    - 특정 파일만을 원하면 지정 가능
  - `commit`: 추가된 파일의 버전을 등록하는 명령어
    - `-m`: 메세지를 추가하는 옵션
  - `push`: 원격 저장소로 보내는 명령어
  - `.gitignore`: 깃이 관리하지 않아도 될 파일을 설정하는 파일

---
[← 목차로](./README.md)