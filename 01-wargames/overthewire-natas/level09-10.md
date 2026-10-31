# Level 9 → 10

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. View sourcecode를 눌러 소스코드 확인
    ```php
    <?
    if($key != "") {
        passthru("grep -i $key dictionary.txt");
    }
    ?>
    ```
    명령어 인젝션 가능 확인 완료
2. `;id;`를 통해 명령어 인젝션이 가능한지 확인 → `uid=30009(natas9) gid=30009(natas9) groups=30009(natas9)`
    - 가능하다는 것을 확인하였으므로 이전 단계 비밀번호 파일 위치를 입력해 비밀번호 취득 
3. `;cat /etc/natas_webpass/natas10;` 입력 → 비밀번호 취득

## 4. 원리 (Theory)
- OS Command Injection
  - 운영체제 명령어 삽입 취약점으로 웹 애플리케이션이 사용자로부터 입력받은 값을 검증이나 필터링 없이 시스템 쉘 명령어의 일부로 실행할 때 발생하는 취약점
- `passthru()`
  - PHP에서 외부 시스템 명령어를 실행하고, 그 결과를 가공하지 않은 상태로 웹 브라우저나 출력 버퍼에 출력하는 내장 함수

---

# 보고서

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | `passthru()` 입력값 미검증으로 인한 OS Command Injection |
| 분류 | CWE-78 (Improper Neutralization of Special Elements used in an OS Command) |
| 대상 | natas9 웹 페이지 (검색 기능) |
| 위험도 | 9.8 (Critical) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` |

## 2. 취약점 설명
- 사용자 입력값(`$key`)이 검증이나 이스케이프 없이 `passthru("grep -i $key dictionary.txt")`에 그대로 삽입되어 쉘 명령으로 실행됨
- `;` 같은 쉘 메타문자를 입력하면 `grep` 뒤에 임의의 명령을 이어서 실행할 수 있음
- 명령은 웹 서버 실행 계정 권한으로 수행됨

## 3. 재현 절차
1. 소스코드 확인 → 입력값이 `passthru()`로 `grep` 명령에 직접 전달되는 것 확인
2. 검색창에 `;id;` 입력 → `id` 명령 결과가 출력되어 명령어 인젝션 가능 확인
3. 비밀번호 파일 위치를 지정해 `cat` 명령을 주입 → 파일 내용 확인

## 4. 영향 (Impact)
- 웹 서버 권한으로 임의 명령 실행이 가능하여 파일 열람, 변경, 삭제 등 시스템 전반에 영향을 줄 수 있음
- 명령어 인젝션은 데이터 변경·서비스 중단까지 가능하므로 기밀성·무결성·가용성 모두 High로 산정
- 검색창 입력만으로 공격이 가능하며 별도의 고급 기법이 필요하지 않아 악용 난이도가 낮음

## 5. 대응 방안 (Remediation)
- 쉘 명령 호출을 피하고 PHP 내부 함수(`file()`, `preg_grep()` 등)로 검색 기능 구현
- 불가피하게 사용할 경우 `escapeshellarg()`로 입력값을 이스케이프
- 허용 문자만 통과시키는 화이트리스트 방식의 입력값 검증 적용
- 웹 서버 실행 계정의 권한을 최소화

## 6. 참고
- [CWE-78](https://cwe.mitre.org/data/definitions/78.html)
- [OWASP Top 10 - A03:2021 Injection](https://top10.owasp.org/2021/A03_2021-Injection/)

---
[← 목차로](./README.md)