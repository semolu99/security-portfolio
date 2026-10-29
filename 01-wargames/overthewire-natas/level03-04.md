# Level 3 → 4

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. 페이지 확인을 위해 화면 우클릭을 통해 `View Page Source` 확인
    ```html
    <h1>natas3</h1>
    <div id="content">
        There is nothing on this page
        <!-- No more information leaks!! Not even Google will find it this time... -->
    </div>
    ```
    중요한 단서로 `Not even Google will find it this time`가 존재함
2. `/robots.txt` 접근
    ```text
    User-agent: *
    Disallow: {경로}
    ```
    경로가 하나 나왔음
3. `/{경로}` 이동
4. `users.txt` 클릭

## 4. 원리 (Theory)
- 로봇 배제 표준
  - 웹사이트에 웹 크롤러 같은 로봇들의 접근을 제어하기 위한 규약
  - 해당 페이지는 누구나 확인이 가능하기에 공격자에게 정보를 줄 수 있는 지점이 될 수 있음

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | robots.txt를 통한 은닉 경로 노출 및 사용자 계정 정보 노출 |
| 분류 | CWE-200 (Exposure of Sensitive Information to an Unauthorized Actor) |
| 대상 | natas3 웹 페이지 |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- `robots.txt`의 `Disallow`는 크롤러에 대한 권고일 뿐 접근 통제가 아니며, 숨기려는 경로를 누구나 확인할 수 있게 공개함
- `Disallow`에 명시된 경로에 디렉토리 리스팅이 활성화되어 있고, `users.txt`에 계정ID:비밀번호가 평문으로 저장되어 인증 없이 열람 가능
- 페이지 소스의 주석("Not even Google will find it")이 오히려 `robots.txt` 확인을 유도하는 단서가 됨

## 3. 재현 절차
1. 브라우저에서 대상 페이지 접속 후 `View Page Source`로 소스 주석 확인
2. `/robots.txt` 접근 → `Disallow`에 명시된 경로 확인
3. 해당 경로로 이동 → 파일 목록 노출 확인
4. `users.txt` 열람 → 계정 정보 확인

## 4. 영향 (Impact)
- 인증 없이 admin 계정 정보가 노출되어 관리자 권한으로 접근 및 정보 열람 가능, 변경 기능은 확인되지 않아 무결성 영향은 낮게 산정
- 별도의 도구나 공격 기법 없이 브라우저만으로 접근 가능하여 악용 난이도가 매우 낮음

## 5. 대응 방안 (Remediation)
- 민감한 경로를 `robots.txt`에 나열하지 않기 (접근 제한 수단으로 사용 금지)
- 민감 경로에는 인증·권한 검사 등 실제 접근 통제 적용
- 디렉토리 리스팅 비활성화 (Apache `Options -Indexes`, nginx `autoindex off`)
- 계정·비밀번호 등 민감 파일을 웹 루트 밖으로 이동하고, 비밀번호는 평문이 아닌 해시로 저장

## 6. 참고
- [CWE-200](https://cwe.mitre.org/data/definitions/200.html)
- [CWE-548](https://cwe.mitre.org/data/definitions/548.html)
- [OWASP Top 10 - A05:2021 Security Misconfiguration](https://top10.owasp.org/2021/A05_2021-Security_Misconfiguration/)

---
[← 목차로](./README.md)