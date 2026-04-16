---
name: pr-create
description: GitHub PR 생성. 작업 완료 후 PR 열어달라고 하거나 feature 브랜치 작업이 끝났을 때 사용.
---

# PR Create

## 순서

### 1. 현재 브랜치 확인
```bash
git branch --show-current
git log origin/dev..HEAD --oneline
```
- 브랜치명에서 이슈 번호 추출: `feature/10-user-profile` → `#10`
- 이슈 번호 없으면 중단하고 issue-create 먼저 안내

### 2. base 브랜치 결정
| 현재 브랜치 | base |
|-------------|------|
| `feature/*` | `dev` |
| `bug/*` | `dev` |
| `chore/*` | `dev` |
| `refactor/*` | `dev` |
| `test/*` | `dev` |
| `dev` | `staging` |
| `staging` | `main` |

### 3. 변경 내역 분석
```bash
git log origin/{base}..HEAD --oneline
git diff origin/{base}...HEAD --stat
```
커밋 내역을 보고 TYPE 결정:
`FEAT` / `FIX` / `REFACTOR` / `TEST` / `DOCS` / `STYLE` / `CHORE` / `PERF`

### 4. PR 내용 작성
**PR 제목 형식**
```
[TYPE] #{issue_number} short-description
예: [FEAT] #10 사용자 프로필 API 개발
```

**PR 본문 형식**
```markdown
## 개요
<!-- 변경 내용 요약 -->

## 작업 내용
- [ ] ...
- [ ] ...

## 관련 이슈
close #{issue_number}
```

### 5. PR 생성
GitHub MCP(`mcp__github__create_pull_request`)로 PR 생성:
- `title`: 위 형식 준수
- `body`: 위 본문 형식 준수
- `head`: 현재 브랜치
- `base`: 결정된 base 브랜치

### 6. 결과 안내
PR URL을 사용자에게 전달. Merge는 사용자가 직접.

## 규칙
- 이슈 번호 없으면 PR 생성 불가
- PR 제목 TYPE은 반드시 대문자
- Merge는 절대 하지 않음 (사람이 직접)
- gh CLI 사용 금지 — `mcp__github__create_pull_request`만 사용
