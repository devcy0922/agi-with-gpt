# 🏗️ PROJECT.md — devcy0922/agi-with-gpt

> 본 문서는 `agi-with-gpt` 레포지토리의 변치 않는 장기 정의, 목적, 아키텍처 및 핵심 제약 사항을 정의하는 SSOT 문서입니다.

---

## 1. Project Overview

- **Repository**: `devcy0922/agi-with-gpt`
- **Repository Root**: `/Users/yooncy/srv/agi-with-gpt`
- **Primary Purpose**: ChatGPT와 Coding Agent(Gemini, Local LLM)를 연결하여 대화 기반 자율 개발 루프(Intake → Plan → Second Inspection → Auto Execution → Test → Deploy → Smoke Test → Doc Update → Archive → Status Summary & Next 3 Actions)를 지속적이고 안정적으로 운영하는 오케스트레이션 환경 구축.

---

## 2. Core Architecture

```text
User (Human Gate)
  │ (Prompt Copy & Paste)
  ▼
Coding Agent (Gemini / Local LLM)
  │ Target Repo Verification & Boundary Guardrails
  ├─ agents/repositories/<repo-name>/ (PROJECT, STATE, ROADMAP, DEPLOYMENT)
  ├─ agents/sessions/active/<session-id>/ (GOAL.md, PLAN.md, state.json)
  ├─ Plan 작성 & Second Inspection (자기 검수)
  ├─ 승인 없이 구현 & 자동 테스트/빌드
  ├─ 실배포 & 배포 후 Smoke Test
  ├─ 레포 4대 문서 업데이트 & Session Archive
  └─ Current Status 요약 & Recommended Next Actions (3개)
```

- **Core Structure**:
  - `agents/rules/`: SSOT 작업 및 보안/격리/자율루프 규칙
  - `agents/repositories/`: 레포지토리별 장기 문서 (`PROJECT`, `STATE`, `ROADMAP`, `DEPLOYMENT`)
  - `agents/workflows/`: 대화 기반 자율 실행 루프 정의
  - `agents/templates/`: 세션 및 레포지토리 4대 문서 표준 템플릿
  - `agents/sessions/`: 활성 및 완료 세션 영속화 (`active/`, `completed/`)

---

## 3. Key Features

- [x] 대화 기반 Autonomous Coding Loop POC 워크플로우
- [x] Target Repository & Filesystem Boundary Isolation
- [x] Shared Infrastructure Protection Guardrail
- [x] Plan v1 → Second Inspection → PLAN Final 자기 검수 단계
- [x] 승인 대기 없는 자율 실행 (Approval-Free Auto Execution)
- [x] 실배포 및 배포 후 Smoke Test 가이드
- [x] 완료 문서 자동 갱신 및 Session Archive 규칙
- [x] 작업 완료 후 Current Status 및 다음 Action 3개 제안 체계

---

## 4. Architectural Constraints & Invariants

- `.env`, 사설 IP, API key 노출 절대 금지 (Zero Tolerance)
- Target Repository 외 타 레포지토리 및 `repository_root` 바깥 파일 수정 절대 금지
- `docker system prune` 등 파괴적 shared infrastructure 명령 사용 금지
- Git Tag는 사용자의 명시적 확정 지시가 있을 때만 생성
