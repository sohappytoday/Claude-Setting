---
name: git-push
description: 브랜치를 확인 후 지정한 원격 브랜치에 push
allowed-tools: Bash(git *)
---

아래 순서대로 진행해:

## 1. 현재 상태 확인

```bash
git status
git log --oneline -5
```

현재 브랜치와 최근 커밋을 사용자에게 보여줘.

## 2. push할 브랜치 확인

$ARGUMENTS가 있으면 그것을 대상 브랜치로 사용.
없으면 사용자에게 반드시 물어봐:

> 어느 브랜치에 push할까요? (현재 브랜치: `<현재 브랜치명>`)

## 3. Push 실행

지정한 브랜치로 push:

```bash
git push origin <대상 브랜치>
```

원격 브랜치 추적이 설정되지 않은 경우 `-u` 옵션 사용:

```bash
git push -u origin <대상 브랜치>
```

## 4. 결과 확인

push 성공 시 결과를 간략히 출력.
실패 시 에러 내용을 사용자에게 알리고 원인 설명.
