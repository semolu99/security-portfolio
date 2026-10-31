# Level 7 → 8

## 1. 목표 (Objective)
페이지 내에서 비밀번호 찾기

## 2. 사용 명령어 (Commands)
`없음`

## 3. 풀이 과정 (Procedure)
1. 페이지에는 home, about만 존재
    - 각각 클릭 시 this is the front page, this is the about page
    - 엔드포인트는 `/index.php?page=home`, `/index.php?page=about`
    - 다른 값을 넣어보기
2. `/index.php?page=abc` 접근
    ```text
    Warning: include(abc): failed to open stream: No such file or directory in /var/www/natas/natas7/index.php on line 21
    Warning:  include(): Failed opening 'abc' for inclusion (include_path='.:/usr/share/php') in /var/www/natas/natas7/index.php on line 21
    ```
    - include() 함수 사용을 확인
3. `view page source` 확인
    - `hint: password for webuser natas8 is in /etc/natas_webpass/natas8` 주석으로 힌트 발견
4. `/index.php?page=../../../../etc/natas_webpass/natas8` 접근
    패스워드 확인

## 4. 원리 (Theory)
- PHP `include()`
  - 다른 PHP 파일이나 HTML 파일을 현재 실행 중인 스크립트에 불러와 포함시키는 명령어
- LFI(Local File Inclusion)
  - 로컬 파일 포함 취약점으로 웹 애플리케이션이 사용자로부터 입력받은 파일 이름을 적절한 검증이나 필터링 없이 그대로 파일 로드 함수에 전달할 때 발생하는 보안 취약점

---

# 보고서
> 워게임의 비밀번호가 노출된 것이 아닌, admin 계정이 노출된 것으로 산정
> 데이터 변경 기능이 확인되지 않아 무결성 영향은 낮게 산정

## 1. 취약점 요약
| 항목 | 내용 |
|------|------|
| 취약점명 | `include()` 파라미터 조작을 통한 LFI(Local File Inclusion) / Path Traversal |
| 분류 | CWE-22 (Path Traversal), CWE-98 (Improper Control of Filename for Include/Require Statement in PHP Program) |
| 대상 | natas7 웹 페이지 (`index.php?page=`) |
| 위험도 | 8.2 (High) |
| 벡터 | `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:L/A:N` |

## 2. 취약점 설명
- `page` 파라미터 값이 검증 없이 `include()`에 전달되어, 공격자가 `../`를 이용해 서버 내 임의의 파일을 불러올 수 있음
- 존재하지 않는 값 입력 시 에러 메시지에 `include()` 사용과 서버 내 파일 경로가 노출되어 공격에 단서를 제공함
- 페이지 소스 주석에 비밀번호 파일의 위치 힌트가 남아 있음

## 3. 재현 절차
1. 대상 페이지의 `home`, `about` 링크에서 `/index.php?page=` 형태의 엔드포인트 확인
2. 존재하지 않는 값(`abc`)을 입력 → `include()` 에러 메시지로 사용 함수와 서버 경로 확인
3. 페이지 소스의 주석에서 비밀번호 파일 위치 힌트 확인
4. `page` 값에 `../`를 포함한 상대 경로로 해당 파일을 지정 → 파일 내용 확인

## 4. 영향 (Impact)
- 웹 서버 권한으로 읽을 수 있는 임의 파일 열람이 가능하여 admin 계정 정보 등 민감 정보 노출, 변경 기능은 확인되지 않아 무결성 영향은 낮게 산정
- URL 파라미터 조작만으로 공격이 가능하며 별도의 고급 기법이 필요하지 않아 악용 난이도가 낮음

## 5. 대응 방안 (Remediation)
- 허용된 페이지 목록(`home`, `about`)만 사용하는 화이트리스트 방식으로 `page` 값 검증
- `../` 등 경로 이동 문자열 차단 및 입력값 정규화
- `open_basedir` 설정으로 PHP가 접근할 수 있는 디렉토리 제한
- `display_errors`를 끄고 에러 메시지에 서버 경로가 노출되지 않도록 설정
- 소스 주석에 민감한 경로·힌트를 남기지 않기

## 6. 참고
- [CWE-22](https://cwe.mitre.org/data/definitions/22.html)
- [CWE-98](https://cwe.mitre.org/data/definitions/98.html)
- [OWASP Top 10 - A01:2021 Broken Access Control](https://top10.owasp.org/2021/A01_2021-Broken_Access_Control/)

---
[← 목차로](./README.md)