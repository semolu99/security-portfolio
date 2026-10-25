# security-portfolio

워게임을 풀며 리눅스와 웹 보안 기초를 학습하고, 풀이 과정과 원리를 기록하는 포트폴리오입니다.
이후 CVE·CWE 분석으로 실제 취약점 연구까지 확장할 예정입니다.
비밀번호와 정답은 기록하지 않습니다.

## 진행 현황
| 플랫폼 | 상태 | 링크 |
|--------|------|------|
| OverTheWire - Bandit | 완료 | [풀이 보기](https://github.com/semolu99/security-portfolio/tree/main/01-wargames/overthewire-bandit) |
| OverTheWire - Natas | 진행 중 | [풀이 보기](https://github.com/semolu99/security-portfolio/tree/main/01-wargames/overthewire-natas) |
| WeChall | 진행 중 | [프로필 보기](https://www.wechall.net/profile/jien) |

세부 진행률은 [01-wargames/README.md](./01-wargames/README.md)에서 확인할 수 있습니다.

## 저장소 구조
```
security-portfolio/
├── 01-wargames/
│   ├── overthewire-bandit/   # 레벨별 풀이 (목표 · 명령어 · 풀이 과정 · 원리)
│   └── overthewire-natas/    # 웹 취약점 풀이 + 보고서 (CWE · CVSS · 대응 방안)
└── 02-vulnerability-research/
    ├── cve/                  # CVE 사례 분석 (CVE마다 폴더 1개)
    └── cwe/                  # CWE (예정)
```