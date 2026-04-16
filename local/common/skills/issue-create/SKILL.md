---
name: issue-create
description: GitHub 이슈 생성. 이슈 만들어달라고 하거나 새 기능/버그/작업 시작 전에 사용.
---

# Issue Create

## 순서
1. 작업 내용을 듣고 적절한 템플릿 선택: Bug / Chore / Feature / Refactor / Test
2. @templates/{선택한템플릿}.md 참고해서 이슈 내용 작성
3. title, labels, Assignee 확인 후 사용자에게 내용 미리 보여주기
4. `mcp__github__create_issue`로 이슈 생성
   - `owner`: 저장소 소유자
   - `repo`: 저장소 이름
   - `title`: 이슈 제목
   - `body`: 템플릿 기반 본문
   - `labels`: 선택한 레이블 배열
   - `assignees`: 담당자 배열
5. 생성된 이슈 번호로 브랜치명 결정: `feature/{issue_number}-{short-description}`
6. `mcp__github__create_branch`로 원격 브랜치 생성
   - `owner`: 저장소 소유자
   - `repo`: 저장소 이름
   - `branch`: `feature/{issue_number}-{short-description}`
   - `from_branch`: `dev`
7. 사용자에게 브랜치 체크아웃 명령 안내:
   ```bash
   git fetch origin
   git switch feature/{issue_number}-{short-description}
   ```

## 규칙
- PR 전에 반드시 이슈 먼저 생성
- 이슈 번호 없으면 PR 불가
- Assignee, Label 반드시 선택
- gh CLI 사용 금지 — MCP 도구만 사용