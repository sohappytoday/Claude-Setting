# Claude-Setting

Claude Code 전역 설정 및 스킬 관리 저장소.

## 구조

- `global/settings.json` — 전역 Claude Code 설정
- `global/skills/` — 커스텀 스킬 정의
- `.claude/settings.local.json` — 로컬 전용 설정 (git 제외, `.env`와 동일하게 취급)

## 주의

- `.claude/settings.local.json`은 `.gitignore`에 등록되어 있으므로 커밋하지 말 것
- 커밋 메시지에 `Co-Authored-By` 추가 금지
