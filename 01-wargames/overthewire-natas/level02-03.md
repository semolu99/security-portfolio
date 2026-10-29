# Level 2 → 3

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. 페이지 확인을 위해 화면 우클릭을 통해 `View Page Source` 확인
    ```html
    <h1>natas2</h1>
    <div id="content">
    There is nothing on this page
    <img src="files/pixel.png">
    ```
    중요한 단서로 `files/pixel.png`가 존재함
2. `/files` 접근
3. `users.txt` 클릭

## 4. 원리 (Theory)
- Directory Listing
  - 웹 서버의 설정 중 디렉토리 인덱싱 기능을 켜놓은 경우 디렉토리에 `index.*`와 같은 기본 문서가 없을 때, 디렉토리에 존재하는 파일과 폴더의 목록을 웹 브라우저에 그대로 노출하는 기능

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | 디렉토리 리스팅을 통한 사용자 계정 정보 노출 |
| 분류 | CWE-548 (Exposure of Information Through Directory Listing) |
| 대상 | natas2 웹 페이지 |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- 웹 서버가 디렉토리 인덱싱(Directory Listing)을 활성화하여 `/files/` 경로의 파일·폴더 목록이 그대로 노출됨
- 페이지 소스의 `files/pixel.png` 경로로 디렉토리 존재를 유추할 수 있고, 해당 디렉토리에 `users.txt`가 방치됨
- `users.txt`에 계정ID:비밀번호가 평문으로 저장되어 있어 인증 없이 열람 가능

## 3. 재현 절차
1. 브라우저에서 대상 페이지 접속 후 `View Page Source`로 `files/pixel.png` 경로 확인
2. 상위 경로 `/files/`에 직접 접근 → 파일 목록 노출 확인
3. `users.txt` 열람 → 계정 정보 확인

## 4. 영향 (Impact)
- 인증 없이 사용자 계정 정보(ID/비밀번호)가 노출되어 admin 계정 무단 접근 가능, 변경 기능은 확인되지 않아 무결성 영향은 낮게 산정
- 별도의 도구나 공격 기법 없이 브라우저만으로 접근 가능하여 악용 난이도가 매우 낮음

## 5. 대응 방안 (Remediation)
- 디렉토리 리스팅 비활성화 (Apache `Options -Indexes`, nginx `autoindex off`)
- 디렉토리마다 기본 문서(`index.html`) 배치
- 계정·비밀번호 등 민감 파일을 웹 루트 밖으로 이동하고, 비밀번호는 평문이 아닌 해시로 저장
- 웹 루트 내 불필요 파일 점검 및 접근 제어 적용

## 6. 참고
- [CWE-548](https://cwe.mitre.org/data/definitions/548.html)
- [OWASP Top 10 - A05:2021 Security Misconfiguration](https://top10.owasp.org/2021/A05_2021-Security_Misconfiguration/)

---
[← 목차로](./README.md)