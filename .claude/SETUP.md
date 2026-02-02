# Claude Settings 공유 설정

이 디렉터리는 workfolio-backend와 workfolio-frontend 프로젝트에서 공유하는 Claude/Cursor 설정을 포함합니다.

## 구조

```
.claude/
├── rules/      # 프로젝트 공통 코딩 규칙 및 가이드라인 (.mdc 파일)
├── skills/     # 재사용 가능한 스킬 정의
├── agents/     # 에이전트 설정 및 구성
├── README.md   # 이 파일
└── AGENTS.md   # 에이전트 설정 문서
```

## 설정된 프로젝트

- `workfolio-backend/.claude` → `../claude-settings/.claude` (심볼릭 링크)
- `workfolio-frontend/.claude` → `../claude-settings/.claude` (심볼릭 링크)

## 사용 방법

### Rules 추가

`.claude/rules/` 디렉터리에 `.mdc` 파일을 추가하세요:

```markdown
---
description: 규칙 설명
globs: **/*.kt  # 파일 패턴 (선택사항)
alwaysApply: false  # 항상 적용 여부
---

# 규칙 제목

규칙 내용...
```

### Skills 추가

`.claude/skills/` 디렉터리에 스킬 파일을 추가하세요.

### Agents 설정

`.claude/agents/` 디렉터리에 에이전트 설정 파일을 추가하거나 `AGENTS.md`를 수정하세요.

## 참고

- 각 프로젝트는 이 공유 설정을 자동으로 참조합니다
- 프로젝트별 특수 설정이 필요한 경우, 각 프로젝트의 `.cursor/` 디렉터리를 사용할 수 있습니다
