# project-md-slim

Claude Code용 CLAUDE.md 경량화 스킬. 무거워진 CLAUDE.md에서 **"항상 적용되는 핵심 규칙"만 남기고**, 상세 내용은 작업유형별 참조 문서(`docs/guide/guide-*.md`)로 분할합니다.

> A Claude Code skill that slims down a heavy CLAUDE.md — keeping only always-applicable rules and splitting details into task-specific reference docs.

## 왜 필요한가

CLAUDE.md는 **모든 세션 시작 시 전부 로드**됩니다. DB 상세, 배포 절차, UI 가이드까지 다 들어 있으면 그 세션에서 안 쓰는 내용까지 매 대화가 짊어집니다. 이 스킬은 다음 기준으로 나눕니다:

> **"이 내용을 모든 세션이 읽어야 하는가?"**
> 그렇다 → CLAUDE.md에 남김 · 아니다 → `docs/guide/guide-<주제>.md`로 이동 + CLAUDE.md에 참조 표만

실제 사례: 294줄 CLAUDE.md → 약 100줄 + 가이드 문서 5종(db·deploy·domain·feature·ui).

## 설치

Claude Code가 설치되어 있어야 합니다.

**Windows (PowerShell)**

```powershell
git clone https://github.com/seamoon23/project-md-slim.git "$env:USERPROFILE\.claude\skills\project-md-slim"
```

**macOS / Linux**

```bash
git clone https://github.com/seamoon23/project-md-slim.git ~/.claude/skills/project-md-slim
```

팀 공유는 프로젝트 저장소의 `.claude/skills/project-md-slim/`에 파일을 복사해 커밋하면 됩니다. 업데이트는 설치 폴더에서 `git pull`.

## 사용법

| 명령 | 동작 | 파일 변경 |
| --- | --- | --- |
| `/project-md-slim check` | 진단만: 섹션별 남김/이동 분류표 + 예상 절감량 보고. 처음이면 이것부터 | 없음 |
| `/project-md-slim` | 진단 → 분할 계획표 제시 → **승인 후에만** 분할 실행 | 승인 후에만 |

## 안전 원칙

1. **수정 범위는 CLAUDE.md 단 하나** — 나머지는 전부 신규 파일 생성. 소스 코드·설정·기존 문서는 절대 건드리지 않음
2. **승인 전 변경 없음** — 분할 계획표를 먼저 제시하고 승인 후에만 실행
3. **내용 유실 없음** — 삭제가 아니라 이동. 원문을 요약·의역하지 않고 그대로 옮김
4. **복구 경로 확보** — git 커밋 상태에서 시작 (git이 없으면 백업 사본 생성 후 진행)
5. **검증 보고** — 전/후 줄 수, 이동 섹션 목록, 원문 대비 유실 대조를 보고에 포함

## 관련 스킬

- [project-workspace-setup](https://github.com/seamoon23/project-workspace-setup) — 저장소 3분할 구조(dev/docs/works) 셋팅. 분리된 가이드가 들어갈 `docs/guide/`를 만들어주는 자매 스킬.

## License

MIT
