# Level 10 → 11

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`grep`, `cat`, 백틱

## 3. 풀이 과정 (Procedure)
1. View sourcecode를 눌러 소스코드 확인
    ```php
    if($key != "") {
        if(preg_match('/[;|&]/',$key)) {
            print "Input contains an illegal character!";
        } else {
            passthru("grep -i $key dictionary.txt");
        }
    }
    ```
    이젠 특수 문자를 필터링해서 이용이 불가능함
    - 사용하지 못하는 특수문자
        - `;`
        - `|`
        - `&`
2. 백틱을 이용한 명령어 선입력을 통한 방법을 이용
    ```bash
    `cat /etc/natas_webpass/natas11` /etc/natas_webpass/natas11
    # 아래와 같이 실행됨
    grep -i `cat /etc/natas_webpass/natas11` /etc/natas_webpass/natas11 dictionary.txt
    ```

## 4. 원리 (Theory)
- `grep`
  - grep의 인자는 2종류로 처음 나온 것은 패턴, 그 후에는 대상 파일을 의미
  - 따라서 패턴을 패스워드로 걸고, 찾는 파일을 하나 더 추가한 방식으로 진행
  - 만일 패턴 인자를 여러 개 두고 싶다면 `-e` 옵션을 사용
- 명령 치환
  - 명령 치환은 명령어 내부에 다른 명령어의 값을 사용하기 위해 사용하는 기법
  - 치환된 명령이 바깥 명령보다 먼저 실행되고, 그 결과가 그 자리에 들어감
  - 백틱과 `$()`를 통해 사용 가능

---

# 보고서

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | `preg_match()` 블랙리스트 필터 우회를 통한 OS Command Injection |
| 분류 | CWE-78 (Improper Neutralization of Special Elements used in an OS Command), CWE-184 (Incomplete List of Disallowed Inputs) |
| 대상 | natas10 웹 페이지 (검색 기능) |
| 위험도 | 9.8 (Critical) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H` |

## 2. 취약점 설명
- 사용자 입력값(`$key`)에 대해 `preg_match('/[;|&]/', $key)`로 `;`, `|`, `&`만 차단하는 블랙리스트 방식의 필터가 적용되어 있음
- 필터에 포함되지 않은 백틱(`` ` ``)이나 `$()` 같은 명령 치환 구문은 그대로 통과하여, 쉘이 `grep` 실행 전에 치환된 명령을 먼저 실행함
- 입력값이 이스케이프 없이 `grep`의 인자로 삽입되므로 패턴뿐 아니라 검색 대상 파일까지 임의로 지정할 수 있음
- 명령은 웹 서버 실행 계정 권한으로 수행됨

## 3. 재현 절차
1. 소스코드 확인 → `;`, `|`, `&`만 차단하는 필터가 존재하고 입력값이 `passthru()`로 `grep` 명령에 직접 전달되는 것 확인
2. 필터에 없는 백틱을 이용해 명령 치환 페이로드 구성 → 비밀번호 파일 내용을 `grep`의 패턴으로, 비밀번호 파일 위치를 검색 대상 파일로 지정
3. 검색창에 페이로드 입력 → 비밀번호 파일 내용 출력 확인

## 4. 영향 (Impact)
- 웹 서버 권한으로 임의 명령 실행이 가능하여 파일 열람, 변경, 삭제 등 시스템 전반에 영향을 줄 수 있음
- 명령어 인젝션은 데이터 변경·서비스 중단까지 가능하므로 기밀성·무결성·가용성 모두 High로 산정
- 검색창 입력만으로 공격이 가능하며 별도의 고급 기법이 필요하지 않아 악용 난이도가 낮음
- 필터가 존재해 방어된 것처럼 보이지만, 블랙리스트에서 누락된 문자로 쉽게 우회되어 실질적인 보호가 되지 않음

## 5. 대응 방안 (Remediation)
- 쉘 명령 호출을 피하고 PHP 내부 함수(`file()`, `preg_grep()` 등)로 검색 기능 구현
- 블랙리스트 대신 허용 문자(예: 영숫자)만 통과시키는 화이트리스트 방식의 입력값 검증 적용
- 불가피하게 쉘 명령을 사용할 경우 `escapeshellarg()`로 입력값을 이스케이프하고, `--`로 옵션과 인자를 구분해 입력값이 추가 인자로 해석되지 않도록 처리
- 웹 서버 실행 계정의 권한을 최소화

## 6. 참고
- [CWE-78](https://cwe.mitre.org/data/definitions/78.html)
- [CWE-184](https://cwe.mitre.org/data/definitions/184.html)
- [OWASP Top 10 - A03:2021 Injection](https://top10.owasp.org/2021/A03_2021-Injection/)
- [OWASP OS Command Injection Defense Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/OS_Command_Injection_Defense_Cheat_Sheet.html)

---
[← 목차로](./README.md)