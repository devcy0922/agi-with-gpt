# AGENTS.md — AGI with GPT Agent SSOT Entrypoint

> **MANDATORY**: 본 파일은 `agi-with-gpt` 레포지토리 및 에이전트 협업 환경의 최우선 규칙 진입점입니다.
> 에이전트는 어떠한 작업, 설계, 코드 변경을 시작하기 전에 이 파일과 `agents/rules/` 하위 규칙들을 반드시 먼저 확인해야 합니다.

---

## 1. 레포지토리 목적 & 대화 기반 Autonomous Coding Loop POC

`agi-with-gpt`는 여러 Coding Agent와 ChatGPT를 연결하여 실제 소프트웨어 프로젝트를 지속적으로 자율 개발하기 위한 **Coding Agent Collaboration Environment**입니다.

현재 단계에서는 GitHub Issue Scheduler / Polling 기능은 보류하고, **"ChatGPT가 생성한 작업 프롬프트를 사용자가 Coding Agent에 입력하면, Coding Agent가 지정된 Target Repository에서 계획 → 구현 → 배포 → 검증 → 문서화 → 세션 아카이브 → 요약 및 다음 Action 3개 추천까지 자율 수행"**하는 대화 기반 Loop POC를 운용합니다.

### 역할 분담 (Collaboration Roles)
- **ChatGPT**: Planner, Reviewer, Architecture Reviewer, Next Action Selector, Release Decision Assistant (결과 검토 및 다음 실행 Prompt 작성).
- **Coding Agent (Gemini / Primary Worker)**: Target Repository 분석, PLAN 작성, PLAN 자기검수 (Second Inspection), 코드 구현, 테스트, 배포, 배포 후 검증, 문서 갱신, Session 아카이브, 상태 요약 및 다음 Action 3개 추천.
- **User**: Human Gate (작업 사이클 중간 승인 없이, 한 사이클 완료 후 결과를 직접 검증하고 ChatGPT에 전달하는 역할).

---

## 2. 규칙 구조 (Rules SSOT)

상세 규칙은 `agents/rules/` 디렉토리에 정의되어 있습니다.

- 📖 [`agents/rules/00-core-rules.md`](agents/rules/00-core-rules.md) — 핵심 원칙, 언어 규정, Prompt 해석 우선순위, 과설계 금지
- 🔒 [`agents/rules/01-security-and-isolation.md`](agents/rules/01-security-and-isolation.md) — 보안 및 격리 가드레일 (Repository Boundary, Filesystem Boundary, Shared Infra Guardrail)
- 🔄 [`agents/rules/02-session-lifecycle.md`](agents/rules/02-session-lifecycle.md) — 세션 수명주기, State Machine, 세션 재사용, Archive 규칙
- 📁 [`agents/rules/03-repository-management.md`](agents/rules/03-repository-management.md) — Target Repository 지정 및 Repository별 4대 문서(PROJECT, STATE, ROADMAP, DEPLOYMENT) 관리
- ⚡ [`agents/rules/04-execution-and-deployment.md`](agents/rules/04-execution-and-deployment.md) — Intake, Plan Second Inspection, 승인 없는 자율 실행, 배포/Smoke Test, 문서 갱신, Next Action 3개 추천, Tag 정책

---

## 3. 핵심 아키텍처 및 세션 실행 상태

- **Repository별 문서**: `agents/repositories/<repo-name>/` (PROJECT, STATE, ROADMAP, DEPLOYMENT)
- **세션 상태 영속화**: `agents/sessions/active/<session-id>/` 및 `agents/sessions/completed/<session-id>/`
- **세션 필수 구성**: `GOAL.md`, `PLAN.md`, `state.json`

---

## 4. 5대 핵심 디렉토리 역할

```text
agents/
    rules       = 어떻게 행동해야 하는가 (가드레일, 보안, 격리, 자율루프 규칙)
    repositories= 레포지토리별 장기 문서 (PROJECT, STATE, ROADMAP, DEPLOYMENT)
    skills      = 무엇을 할 수 있는가 (재사용 스킬 및 지침)
    templates   = 무엇을 어떤 형식으로 기록하는가 (GOAL/PLAN/State/RepoDocs)
    workflows   = 어떤 순서로 움직이는가 (대화 기반 Autonomous Loop)
    sessions    = 지금 실제로 무엇을 하고 있는가 (active & completed)
```
