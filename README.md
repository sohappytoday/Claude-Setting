# 클로드 세팅용 레포지토리

---

## global
```bash
~/.claude
```
클로드에 대한 모든 설정을 건드립니다.

### CLAUDE.md
모든 프로젝트에 공통 적용되는 개인 지침 파일.
언어 설정, 코딩 스타일, 개인 선호 규칙 등을 작성합니다.

### settings.json
모델 선택, 환경변수, 권한, Hooks 등 Claude Code의 동작을 제어하는 설정 파일.

### skills/
멀티스텝 워크플로우를 저장하는 디렉토리.
`/skill-name`으로 직접 호출하거나 자동 로드됩니다.
예: PR 올리기, 배포 전 체크리스트

### commands/
자주 쓰는 단일 액션을 단축키로 저장하는 디렉토리.
skills가 멀티스텝이라면, commands는 단일 액션입니다.

### rules/
파일 패턴(glob)별로 다른 규칙을 적용하는 디렉토리.
예: `*.java`는 Java 컨벤션, `*.ts`는 TypeScript 컨벤션

### agents/
도메인 전문 서브에이전트를 정의하는 디렉토리.
예: backend 전문 에이전트, DevOps 에이전트

---

## local

```bash
각 프로젝트/.claude
```
global과 구조가 거의 동일하나, 아래 두 파일이 추가됩니다.

### CLAUDE.md
프로젝트 전용 지침 파일. 팀원과 git으로 공유합니다.
프로젝트 구조, 기술 스택, 팀 컨벤션 등을 작성합니다.

### CLAUDE.local.md
나만의 프로젝트 메모용 파일. `.gitignore`에 추가해 git에 올리지 않습니다.

### settings.json
프로젝트 전용 설정 파일. 팀원과 git으로 공유합니다.

### settings.local.json
나만의 프로젝트 오버라이드 설정 파일. `.gitignore`에 추가해 git에 올리지 않습니다.