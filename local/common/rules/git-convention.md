---
glob: "**/*"
---

# Git 컨벤션

## 브랜치
- `main` : 운영 서버 (직접 push 불가, PR로만)
- `staging` : 프론트엔드 연동 실험용
- `dev` : 개인 코드 통합
- `feature/{이슈번호}-{작업명}` : 이슈별 개인 브랜치 (예: `feature/1-ci-setup`)

**규칙**
- `main`, `dev`, `staging`은 로컬 직접 push 금지 → PR만 가능
- CI 돌기 전 Merge 금지, CI/Static analysis 실패 시 Merge 금지

**작업 시작 전 필수 절차**
1. 반드시 issue-create skill로 이슈 먼저 생성
2. 이슈 템플릿의 브랜치 생성 규칙 참고해서 브랜치 생성
3. 생성한 브랜치로 체크아웃 후 작업 시작

## 커밋 (AngularJS Commit Convention)
| 타입 | 설명 |
|------|------|
| feat | 새로운 기능 추가 |
| fix | 버그 및 오타 수정 |
| refactor | 리팩토링 |
| test | 테스트 코드 |
| docs | 문서 수정, 주석 추가/삭제 |
| style | 포맷, 세미콜론, 공백 |
| chore | 설정, 패키지, 빌드, CI, CD |
| perf | 성능 개선 |

**규칙**
- 제목은 영어, 첫 단어 대문자 (예: `feat: Add user profile API`)
- 특수 기호 자제, 제목 길면 축약 후 본문에 상세 작성

## PR

**PR 제목**
```
[TYPE] #{issue_number} short-description
예: [FEAT] #10 프로필 포트폴리오 개발
```
- 타입 대문자 고정
- 이슈 번호 없으면 PR 불가

## CI/CD
| 브랜치 | CI | CD |
|--------|----|----|
| `feature/~` | X | X |
| `dev` | PR 시 | X |
| `staging` | PR 시 | Merge 시 |
| `main` | PR 시 | Merge 시 |

**CI 파일**
- `feature` → `dev` : `dev-pr-ci.yml`
- `dev` → `staging` : `staging-pr-ci.yml`
- `staging` → `main` : `main-pr-ci.yml`

**CD 파일**
- `dev` → `staging` : `staging-merge.cd.yml` (Docker 빌드/push → pull → 헬스체크 → 실패 시 롤백)
- `staging` → `main` : `main-merge.cd.yml`
