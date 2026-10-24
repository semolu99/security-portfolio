# Bandit (OverTheWire)

## 1. 개요 (Overview)
- 목적: 워게임 형식으로 리눅스 CLI 기초를 학습
- 링크: https://overthewire.org/wargames/bandit/
- 접속: `ssh bandit0@bandit.labs.overthewire.org -p 2220`
- 환경: Oracle VirtualBox / Ubuntu 26.04.1 LTS

## 2. 사전 지식 (Prerequisites)
- 시작 시점: 기본 리눅스 명령어만 알고 시작

## 3. 레벨 목록 (Level Index)
| Level | 주제 | 핵심 명령어 |
|-------|------|-------------|
| [0 → 4](./level00-04.md) | SSH 접속, 파일 읽기, 파일 목록 보기 | `ssh`, `cat`, `ls` |
| [4 → 7](./level04-07.md) | 파일 찾기 | `file`, `find` |
| [7 → 10](./level07-10.md) | 문자열 찾기 | `grep`, `strings` |
| [10 → 13](./level10-13.md) | 디코딩, 압축 복원 | `base64`, `tr`, `xxd`, `gzip`, `bzip2`, `tar` |
| [13 → 17](./level13-17.md) | SSH 및 네트워크 | `ssh -i`, `nc`, `openssl`, `nmap` |
| [17 → 18](./level17-18.md) | 두 파일의 차이 비교 | `diff` |
| [18 → 19](./level18-19.md) | .bashrc 로그아웃 우회 | `ssh <command>` |
| [19 → 21](./level19-21.md) | setuid | - |
| [21 → 24](./level21-24.md) | 쉘 스크립트 | `/etc/cron.d`, `md5sum`, 쉘 스크립트, `chmod` |
| [24 → 25](./level24-25.md) | 브루트포스 | 쉘 반복문, `nc` |
| [25 → 26](./level25-26.md) | 제한된 쉘 탈출 | `more`, `vim` |
| [26 → 27](./level26-27.md) | setuid 바이너리 활용 | - |
| [27 → 32](./level27-32.md) | Git | `git` |
| [32 → 33](./level32-33.md) | 대문자 변환 쉘 탈출 | - |
| [33 → 34](./level33-34.md) | 마무리 | - |

## 4. 학습 요약 (Summary)
- 배운 점: Git과 리눅스 명령어를 익히는 데 도움을 주었음. 또한 setuid 취약점과 리버스 쉘 등의 원리를 이해하는 데 도움을 받음.

## 5. 유의사항 (Notice)
- 비밀번호/정답은 기록하지 않음 (OverTheWire 정책). 풀이 과정과 원리만 서술.