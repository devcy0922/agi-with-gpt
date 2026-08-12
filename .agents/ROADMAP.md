# 🗺️ ROADMAP.md — devcy0922/agi-with-gpt

> 본 문서는 `agi-with-gpt` 레포지토리의 장기 개발 및 검증 계획을 관리하는 SSOT 문서입니다.

---

## 1. Roadmap Overview

- **Current Milestone**: Phase 1 — Autonomous Coding Loop Architecture & Multi-Repo Isolation Fix

---

## 2. Milestone Details

### Completed Milestone (Phase 1.1 — POC Foundation)
- [x] Autonomous Coding Loop Specification (`AGENTS.md`, `agents/rules/`, `agents/workflows/`)
- [x] Session State Machine & Approval-Free Execution Loop
- [x] Plan 작성 및 Second Inspection 2차 검수 절차
- [x] Deployment / Smoke Test Contract 규정
- [x] Initial POC Session Archive & Git Push

### Current Milestone (Phase 1.2 — Multi-Repo State Ownership Boundary Fix)
- [x] Control Repo (`agi-with-gpt`) vs Target Repo (`govail`, `promptia`, `agi-with-gpt`) Ownership separation
- [x] Control Plane Read-Only Invariant during Target Execution
- [x] Target Repository internal `.agents/` structure adoption (`PROJECT`, `STATE`, `ROADMAP`, `DEPLOYMENT`, `sessions/`)
- [x] Bootstrap workflow for fresh Target Repositories
- [x] Guardrail Enforcement Level classification policy

### Next Milestone (Phase 1.3 — Real Target Repository E2E Verification)
- [ ] GoVail Target Repository (`/Users/yooncy/srv/govail`) Autonomous Coding Loop E2E Real Test
- [ ] GoVail internal `.agents/` bootstrap and active session execution
- [ ] Write Path & Isolation Guardrail Verification during real GoVail task

### Future Milestones (Phase 2 & 3)
- [ ] Local LLM (mlx-lm / vLLM gateway) fallback & hybrid inference integration
- [ ] Polling / Webhook Task Scheduler integration
- [ ] Autonomous Agent Dashboard (optional)
