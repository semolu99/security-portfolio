# Level 8 → 9

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. View sourcecode를 눌러 소스코드 확인
    ```php
    <?
    $encodedSecret = "3d3d516343746d4d6d6c315669563362";

    function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
    }
    ...
    ?>
    ```
    중요한 부분 확인 완료
2. CyberChef 사용
    - 역순으로 디코딩을 진행해, Secret값 확인
3. 값을 입력하고 Submit 진행

## 4. 원리 (Theory)
- CyberChef
  - 여러 인코더와 디코더를 모아놓은 Tool로 복합적으로 인코딩된 값을 복원할 때 유용
- Base64 인코딩
  - 64진법으로 컴퓨터가 사용하는 2진 데이터를 화면에 표시할 수 있는 안전한 64개의 문자로 이루어진 텍스트로 치환
- `bin2hex`
  - 원본 데이터(파일, 문자 등)의 각 바이트를 각각 2자리의 16진수 문자열로 변환
- `strrev`
  - 문자열을 뒤집는 것

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | 소스코드 내 가역 인코딩된 시크릿 노출 |
| 분류 | CWE-261 (Weak Encoding for Password), CWE-540 (Inclusion of Sensitive Information in Source Code) |
| 대상 | natas8 웹 페이지 |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- 비교용 시크릿이 암호화나 해시가 아닌 `base64_encode`, `strrev`, `bin2hex`의 조합으로만 가려진 채 소스코드에 포함되어 있음
- 인코딩 절차가 소스코드에 그대로 공개되어 있고 모두 가역적인 변환이므로 역순으로 디코딩하면 원본 시크릿을 복원할 수 있음

## 3. 재현 절차
1. 소스코드 확인 → 인코딩된 시크릿과 `encodeSecret()` 함수(`base64_encode` → `strrev` → `bin2hex`) 확인
2. CyberChef에서 역순으로 디코딩(`From Hex` → 문자열 뒤집기 → `From Base64`) → 원본 시크릿 확인
3. 확인한 값을 입력창에 넣고 제출 → admin 계정 정보 확인

## 4. 영향 (Impact)
- 시크릿 복원으로 admin 계정 정보 열람 가능, 변경 기능은 확인되지 않아 무결성 영향은 낮게 산정
- 공개된 도구(CyberChef)만으로 공격이 가능하며 별도의 고급 기법이 필요하지 않아 악용 난이도가 낮음

## 5. 대응 방안 (Remediation)
- 시크릿을 소스코드에 하드코딩하지 않고 환경 변수나 별도 비밀 관리 수단으로 관리
- 비교가 목적이라면 인코딩이 아닌 솔트가 적용된 해시(`password_hash`, `password_verify`)를 사용
- 서버 측 소스코드가 외부에 노출되지 않도록 접근 통제

## 6. 참고
- [CWE-261](https://cwe.mitre.org/data/definitions/261.html)
- [CWE-540](https://cwe.mitre.org/data/definitions/540.html)
- [OWASP Top 10 - A02:2021 Cryptographic Failures](https://top10.owasp.org/2021/A02_2021-Cryptographic_Failures/)

---
[← 목차로](./README.md)