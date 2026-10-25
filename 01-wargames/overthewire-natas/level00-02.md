# Level 0 → 1

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. 페이지 확인을 위해 화면 우클릭을 통해 `View Page Source` 확인

## 4. 원리 (Theory)
- `View Page Source`
  - 주석등은 브라우저 화면에 그려지지 않을 뿐, 응답에는 담겨 온다.
  - 페이지를 우클릭해 `View Page Source`(원본 보기)를 통해서 확인이 가능

---

# Level 1 → 2

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. 페이지 우클릭 → `right clicking has been blocked!` 알림 발생
2. 페이지 확인을 위해 `F12`를 통해 확인해 보기
3. `Inspector`에는 딱히 뭐가 나오지 않음
4. `Debugger > Sources`를 통해 소스 확인

## 4. 원리 (Theory)
- `Sources`
  - 클라이언트 `JavaScript`로 막은 것이라 우회 가능
    - `Debugger`우측 톱니바퀴 모양을 클릭 후 `Disable JavaScript` 항목을 선택하면 우회 가능
  - 개발자 도구를 통해서도 확인이 가능

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | 클라이언트 측 주석에 민감 정보 노출 (Information Exposure Through Comments) |
| 분류 | CWE-615 (Inclusion of Sensitive Information in Source Code Comments) |
| 대상 | natas0 웹 페이지, natas1 웹 페이지 |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- 페이지의 HTML 소스 내 주석에 ADMIN 권한의 계정 정보가 평문으로 포함되어 있음
- 주석은 화면에는 렌더링되지 않지만, 응답 본문에는 그대로 포함되어 클라이언트에 전달됨
- 따라서 누구나 `우클릭 > View Page Source` 혹은 `F12` → `Debugger > Sources`로 확인 가능

## 3. 재현 절차
1. 브라우저에서 대상 페이지 접속
2. 우클릭 → `View Page Source` 혹은 `F12` → `Debugger > Sources`
3. HTML 주석(`<!-- -->`) 내용 확인

## 4. 영향 (Impact)
- ADMIN 계정 무단 접근 및 정보 열람 가능, 변경 기능은 확인되지 않음
- 별도의 도구나 공격 기법 없이 접근 가능하여 악용 난이도가 매우 낮음

## 5. 대응 방안 (Remediation)
- 비밀번호, 키, 토큰 등 민감 정보를 소스 코드·주석에 포함하지 않기
- 배포 전 주석 제거 (빌드 단계에서 minify / 주석 제거 적용)
- 민감 정보는 서버 측 환경 변수나 비밀 관리 도구로 분리
- 코드 리뷰 및 시크릿 스캐닝 도구(예: gitleaks)로 점검

## 6. 참고
- [CWE-615](https://cwe.mitre.org/data/definitions/615.html)
- [OWASP Top 10 - A05:2021 Security Misconfiguration](https://top10.owasp.org/2021/A05_2021-Security_Misconfiguration/)

---
[← 목차로](./README.md)