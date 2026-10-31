# Level 6 → 7

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. View sourcecode를 눌러 소스코드 확인
    ```php
    <?
    include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
        } else {
            print "Wrong secret";
        }
    }
    ?>
    ```
    비밀번호가 필요하다는 점을 확인
    비밀번호는 `includes/secret.inc` 여기에 있다는 것으로 판단
    접근이 가능한지 확인
2. `/includes/secret.inc` 접근
    - `$secret` 확인 가능
3. 페이지의 입력칸에 값을 복사해 넣기

## 4. 원리 (Theory)
- `include` 파일(.inc) 확장자
  - 소스코드 중 반복 사용되는 공통 모듈, 데이터베이스 연결 정보, 환경 설정, 비밀번호나 API키 같은 민감한 변수를 모아두는 파일
  - 웹 서버 설정이 미흡할 경우, 사용자가 주소창을 통해 경로를 직접 입력하면 소스코드 내용을 그대로 브라우저에 노출

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | 웹 루트 내 비밀 값 파일(`secret.inc`) 직접 접근 |
| 분류 | CWE-538 (Insertion of Sensitive Information into Externally-Accessible File or Directory) |
| 대상 | natas6 웹 페이지 |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- 인증에 사용되는 비밀 값(`$secret`)이 웹 루트 하위의 `includes/secret.inc` 파일에 저장되어 있으며, URL로 직접 접근하면 파일 내용이 그대로 노출됨
- 페이지 소스코드에서 해당 파일의 경로가 확인되어 공격자가 노출 위치를 쉽게 추측할 수 있음

## 3. 재현 절차
1. 대상 페이지에서 소스코드를 확인하여 `includes/secret.inc`를 불러온다는 점 확인
2. `/includes/secret.inc`로 직접 접근 → `$secret` 값 확인
3. 확인한 값을 입력창에 넣고 제출 → admin 계정 정보 확인

## 4. 영향 (Impact)
- 비밀 값 노출로 admin 계정 정보 열람 가능, 변경 기능은 확인되지 않아 무결성 영향은 낮게 산정
- URL 직접 접근만으로 공격이 가능하며 별도의 고급 기법이 필요하지 않아 악용 난이도가 낮음

## 5. 대응 방안 (Remediation)
- 비밀 값 파일을 웹 루트 밖에 저장하여 URL로 직접 접근할 수 없도록 구성
- 웹 서버 설정으로 `includes/` 디렉토리 및 `.inc` 확장자에 대한 직접 접근 차단
- include 파일 확장자를 `.php`로 사용하여 서버에서 실행되도록 처리
- 비밀 값은 소스코드 대신 환경 변수나 별도 비밀 관리 수단으로 관리

## 6. 참고
- [CWE-538](https://cwe.mitre.org/data/definitions/538.html)
- [OWASP Top 10 - A05:2021 Security Misconfiguration](https://top10.owasp.org/2021/A05_2021-Security_Misconfiguration/)

---
[← 목차로](./README.md)