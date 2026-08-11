# AGENTS.md — AGI with GPT Agent SSOT Entrypoint

> **MANDATORY**: 본 파일은 `agi-with-gpt` 레포지토리 및 에이전트 협업 환경의 최우선 규칙 진입점입니다.
> 에이전트는 어떠한 작업, 설계, 코드 변경을 시작하기 전에 이 파일과 `agents/rules/` 하위 규칙들을 반드시 먼저 확인해야 합니다.

---

## 1. 레포지토리 목적 & 에이전트 역할 분담

`agi-with-gpt`는 여러 Coding Agent와 ChatGPT를 연결하여 실제 소프트웨어 프로젝트를 지속적으로 개발하기 위한 **Coding Agent Collaboration Environment**입니다.

### 역할 분담 (Collaboration Roles)
- **ChatGPT**: 기획, 다음 작업 판단, GitHub Issue 생성, PLAN/GOAL Review, 구현 Review, PR Review 및 승인 (Approval).
- **Gemini (Primary Coding Worker)**: Repository 조사, 세션 생성 및 계획 수립, 구현, Test / Build / Verification, PR 생성, Review 반영.
- **Local LLM (Secondary Coding Worker)**: 반복적인 코드 작업, 코드 분석, 보조 구현, 테스트, local verification.

---

## 2. 규칙 구조 (Rules SSOT)

상세 규칙은 `agents/rules/` 디렉토리에 정의되어 있습니다.

- 📖 [`agents/rules/00-core-rules.md`](agents/rules/00-core-rules.md) — 아키텍처, 역할 분담, 언어 및 기본 원칙
- 🔒 [`agents/rules/01-security-and-isolation.md`](agents/rules/01-security-and-isolation.md) — 보안 가드레일 (`.env` 절대 금지, 사설 IP / API key 유출 금지)
- 🔄 [`agents/rules/02-session-lifecycle.md`](agents/rules/02-session-lifecycle.md) — 세션 생성, 상태 영속화, `<target-repo>-issue-<github-number>` 규정

---

## 3. 세션 실행 상태 (Session Execution State)

별도의 `.ai/state` 디렉토리를 두지 않으며, 모든 작업 실행 상태는 `agents/sessions/` 하위에서 영속 관리됩니다.

- 활성 세션: `agents/sessions/active/<session-id>/`
- 완료 세션: `agents/sessions/completed/<session-id>/`

각 세션 디렉토리 필수 구성:
- `GOAL.md`
- `PLAN.md`
- `state.json`

---

## 4. 5대 핵심 디렉토리 역할

```text
agents/
    rules       = 어떻게 행동해야 하는가 (가드레일 & 보안)
    skills      = 무엇을 할 수 있는가 (재사용 기능 & 지침)
    templates   = 무엇을 어떤 형식으로 기록하는가 (GOAL/PLAN/State/Issue/PR)
    workflows   = 어떤 순서로 움직이는가 (Collaboration & Review Loop)
    sessions    = 지금 실제로 무엇을 하고 있는가 (active & completed)
```
