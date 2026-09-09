# project-md-slim

Claude Code용 지침 문서 경량화 스킬. 무거워진 `CLAUDE.md`(또는 `PROJECT.md`)에서 **"항상 적용되는 핵심 규칙"만 남기고**, 상세 내용은 작업유형별 참조 문서(`docs/guide/guide-*.md`)로 분할합니다.

> A Claude Code skill that slims down a heavy CLAUDE.md (or PROJECT.md) — keeping only always-applicable rules and splitting details into task-specific reference docs.

## 왜 필요한가

`CLAUDE.md`와 `PROJECT.md`는 **모든 세션 시작 시 전부 로드**됩니다. DB 상세, 배포 절차, UI 가이드까지 다 들어 있으면 그 세션에서 안 쓰는 내용까지 매 대화가 짊어집니다. 이 스킬은 다음 기준으로 나눕니다:

> **"이 내용을 모든 세션이 읽어야 하는가?"**
> 그렇다 → 지침 문서에 남김 · 아니다 → `docs/guide/guide-<주제>.md`로 이동 + 지침 문서에 참조 표만

**이동 금지 코어**: "절대 규칙" 절과 "환경"(개발/운영 대비) 표는 어떤 경우에도 분할하지 않습니다 — 매 세션 켜져 있어야 하는 가드레일이므로 guide로 밀어내지 않습니다.

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

## 어느 파일을 분할하나

`project-workspace-setup`으로 셋팅한 저장소는 지침이 3계층으로 나뉘어 있습니다. 이때 **분할 대상은 `PROJECT.md`이고, `CLAUDE.md`는 건드리지 않습니다.**

| 파일 | 로드 | 이 스킬의 처리 |
| --- | --- | --- |
| `CLAUDE.md` | 항상 | **대상 아님** — 이전 작업자의 문서. 최상단 `@PROJECT.md` 진입 블록도 그대로 둠 |
| `PROJECT.md` | 항상 | **분할 대상** — 여기서 상세를 빼고 참조 표를 남김 |
| `docs/guide/*.md` | 필요시만 | 분할 산출물 |

`PROJECT.md`가 없는 일반 저장소라면 `CLAUDE.md`가 분할 대상입니다. 어느 쪽인지 애매하면 사용자에게 묻고, 오래된 프로젝트의 `CLAUDE.md`에 규칙이 섞여 있어도 **강제로 옮기지 않습니다**.

### ⚠ `@경로` 자동 임포트 주의

지침 문서 안의 `@docs/guide/guide-db.md` 같은 **`@` 경로는 Claude Code가 매 세션 자동 로드**합니다. 분할해놓고 참조 표에 `@`를 그대로 쓰면 결국 전부 다시 로드되어 경량화가 무의미해집니다. 참조 표는 일반 경로나 백틱 코드 스팬으로 씁니다.

반대로 `CLAUDE.md` 최상단의 `@PROJECT.md`는 **반드시 유지**합니다. 지우거나 백틱으로 감싸면 절대 규칙이 매 세션 로드되지 않습니다.

## 안전 원칙

1. **수정 범위는 지침 문서 단 하나** — `PROJECT.md`(또는 `CLAUDE.md`) 하나만 수정하고 나머지는 전부 신규 파일 생성. 소스 코드·설정·기존 문서는 절대 건드리지 않음
2. **승인 전 변경 없음** — 분할 계획표를 먼저 제시하고 승인 후에만 실행
3. **내용 유실 없음** — 삭제가 아니라 이동. 원문을 요약·의역하지 않고 그대로 옮김
4. **복구 경로 확보** — git 커밋 상태에서 시작 (git이 없으면 백업 사본 생성 후 진행)
5. **검증 보고** — 전/후 줄 수, 이동 섹션 목록, 원문 대비 유실 대조를 보고에 포함

## 관련 스킬 (project-* 세트)

- [project-workspace-setup](https://github.com/seamoon23/project-workspace-setup) — 저장소 3분할 구조(dev/docs/works) 셋팅 + `PROJECT.md` 생성. 분리된 가이드가 들어갈 `docs/guide/`를 만들어주는 자매 스킬.
- [project-artifacts](https://github.com/seamoon23/project-artifacts) — SI 표준 산출물(테이블·컬럼·코드정의서, 프로그램설명서, ERD) 생성/갱신 스킬.
- [project-dev-tomcat](https://github.com/seamoon23/project-dev-tomcat) — 프로젝트별 로컬 Tomcat 인스턴스를 만들고(포트 자동 분리) 빌드·배포·기동·로그를 `tc` 한 명령으로 관리. Windows 전용.

## License

MIT
