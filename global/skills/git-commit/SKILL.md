---
name: git-commit
description: fetch/merge로 원격과 동기화 후 AngularJS Commit Convention으로 커밋
allowed-tools: Bash(git *)
---

아래 순서대로 진행해:

## 1. 원격과 동기화

```bash
git fetch
```

현재 브랜치를 확인하고 해당 브랜치의 원격과 merge:

```bash
git merge origin/$(git branch --show-current)
```

merge 충돌이 발생하면 사용자에게 알리고 중단.

## 2. 변경사항 확인

```bash
git status
git diff
```

## 3. 스테이징

변경된 파일을 사용자에게 보여주고, 전체 스테이징:

```bash
git add .
```

## 4. 커밋 메시지 작성 (AngularJS Commit Convention)

형식:
```
<type>(<scope>): <subject>

<body>

<footer>
```

### type (필수)
| type | 설명 |
|------|------|
| feat | 새로운 기능 추가 |
| fix | 버그 수정 |
| docs | 문서 변경 |
| style | 코드 포맷 변경 (로직 변경 없음) |
| refactor | 리팩토링 |
| perf | 성능 개선 |
| test | 테스트 추가/수정 |
| chore | 빌드, 설정 등 기타 변경 |
| revert | 이전 커밋 되돌리기 |

### 규칙
- subject: 50자 이내, 소문자로 시작, 마침표 없음, 명령형 현재시제
- body: 변경 이유와 내용 설명 (선택)
- footer: Breaking change 또는 이슈 참조 (선택)

$ARGUMENTS가 있으면 그것을 커밋 메시지로 사용. 없으면 변경 내용을 분석해서 적절한 메시지를 제안하고 사용자에게 확인 후 커밋.

## 5. 커밋 결과 확인

```bash
git log -1 --stat
```
