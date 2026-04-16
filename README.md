# Claude Code 설정 관리 레포지토리

Claude Code의 전역/프로젝트 설정을 버전 관리하고 동기화하는 레포지토리.

---

## 구조 개요

```
.
├── global/          # ~/.claude 에 복사해서 사용 (전역 설정)
├── local/
│   ├── common/      # 모든 프로젝트/.claude 에 복사해서 사용 (공통 프로젝트 설정)
│   └── claude-setting/  # 이 레포 전용 로컬 설정
└── .mcp.json        # MCP 서버 설정 (GitHub 등)
```

> `.claude/` 디렉토리는 `.gitignore`에 등록되어 있어 git에 올라가지 않습니다.
> `global/` → `~/.claude`, `local/common/` → `각 프로젝트/.claude` 로 수동 동기화합니다.

---

## global/

`~/.claude` 에 대응하는 전역 설정.

| 파일/디렉토리 | 설명 |
|---|---|
| `CLAUDE.md` | 모든 프로젝트에 공통 적용되는 개인 지침 (언어, 행동 규칙, 답변 스타일) |
| `settings.json` | 모델, 환경변수, 권한, Hooks 등 Claude Code 전역 동작 설정 |
| `skills/git-commit/` | fetch/merge 동기화 후 AngularJS 컨벤션으로 커밋 |
| `skills/git-push/` | 브랜치 확인 후 원격에 push |

---

## local/common/

모든 프로젝트의 `.claude/` 에 공통으로 복사하는 설정.

| 파일/디렉토리 | 설명 |
|---|---|
| `rules/git-convention.md` | 브랜치, 커밋, PR, CI/CD 규칙 |
| `rules/java-convention.md` | Java 코드 작성 규칙 (`**/*.java` 자동 적용) |
| `rules/python-convention.md` | Python 코드 작성 규칙 (`**/*.py` 자동 적용) |
| `skills/issue-create/` | GitHub 이슈 생성 + 원격 브랜치 생성 (MCP 사용) |
| `skills/pr-create/` | GitHub PR 생성 (MCP 사용) |
| `.mcp.json` | 프로젝트용 MCP 서버 설정 |

---

## 워크플로우

Claude에게 작업을 위임할 때의 전체 흐름:

```
1. 작업 지시
      ↓
2. /issue-create  →  mcp__github__create_issue (이슈 생성)
                 →  mcp__github__create_branch (원격 브랜치 생성)
                 →  사용자가 git switch 로 체크아웃  ← 유일한 수동 단계
      ↓
3. Claude가 코드 수정 (Edit/Write)
      ↓
4. /git-commit  →  git fetch / merge / add / commit
      ↓
5. /git-push    →  git push origin {브랜치}
      ↓
6. /pr-create   →  mcp__github__create_pull_request (PR 생성)
```

> GitHub 관련 작업(이슈/브랜치/PR)은 **MCP**를 사용하며, gh CLI는 사용하지 않습니다.

---

## MCP 설정

`.mcp.json`에서 GitHub MCP 서버를 설정합니다.
토큰에 필요한 권한: **Issues (Read/Write)**, **Contents (Read/Write)**, **Pull Requests (Read/Write)**