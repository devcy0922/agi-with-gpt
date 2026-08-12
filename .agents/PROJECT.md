# 🏗️ PROJECT.md — devcy0922/agi-with-gpt

> 본 문서는 `agi-with-gpt` 레포지토리의 변치 않는 장기 정의, 목적, 아키텍처 및 핵심 제약 사항을 정의하는 SSOT 문서입니다.

---

## 1. Project Overview

- **Repository**: `devcy0922/agi-with-gpt`
- **Repository Root**: `/Users/yooncy/srv/agi-with-gpt`
- **Primary Purpose**: ChatGPT와 Coding Agent(Gemini, Local LLM)를 연결하여 대화 기반 자율 개발 루프(Intake → Plan → Second Inspection → Auto Execution → Test → Deploy → Smoke Test → Doc Update → Archive → Status Summary & Next 3 Actions)를 지속적이고 안정적으로 운영하는 오케스트레이션 및 Control Plane 환경 구축.

---

## 2. Core Architecture (Control Plane vs Target Repository State Ownership)

```text
Control Repository (agi-with-gpt)
  ├── agents/rules/       ← 어떻게 일할 것인가 (Read-Only during execution)
  ├── agents/templates/   ← 문서/세션 템플릿 (Read-Only during execution)
  ├── agents/skills/      ← 공통 재사용 스킬 (Read-Only during execution)
  └── agents/workflows/   ← 공통 실행 순서 (Read-Only during execution)

Target Repository (e.g. govail, promptia, agi-with-gpt)
  └── .agents/            ← 가변 상태 (Mutable State & Execution History)
      ├── PROJECT.md      ← 프로젝트 정의
      ├── STATE.md        ← 현재 실제 구동/구현 상태
      ├── ROADMAP.md      ← 프로젝트 로드맵
      ├── DEPLOYMENT.md   ← 배포/토폴로지 정보
      └── sessions/       ← 작업 세션 영속화 (active / completed)
```

- **Core Structure**:
  - `agents/rules/`: SSOT 작업 및 보안/격리/자율루프 규칙 (Control Plane, Read-Only)
  - `agents/templates/`: 세션 및 레포지토리 4대 문서 표준 템플릿 & Bootstrap 양식
  - `agents/workflows/`: 대화 기반 자율 실행 루프 및 Bootstrap 정의
  - `.agents/`: Target Repository가 자신의 장기 4대 문서 및 세션 이력을 소유하는 디렉토리 (State Owner)

---

## 3. Key Features

- [x] 대화 기반 Autonomous Coding Loop POC 워크플로우
- [x] Control Repo vs Target Repo Ownership Isolation (Multi-Repo Boundary Fix)
- [x] Target Repository & Filesystem Boundary Isolation
- [x] Shared Infrastructure Protection Guardrail
- [x] Plan v1 → Second Inspection → PLAN Final 자기 검수 단계
- [x] 승인 대기 없는 자율 실행 (Approval-Free Auto Execution)
- [x] 실배포 및 배포 후 Smoke Test 가이드
- [x] Target Repo내 `.agents/` 완료 문서 자동 갱신 및 Session Archive 규칙
- [x] Guardrail Enforcement Level 명시 체계 (`POLICY_DEFINED`, `DETERMINISTIC_CHECK_AVAILABLE`, `RUNTIME_ENFORCED`, `E2E_VERIFIED`)

---

## 4. Architectural Constraints & Invariants

- `.env`, 사설 IP, API key 노출 절대 금지 (Zero Tolerance)
- Target Repository 작업 시 Control Repository `agi-with-gpt`에 어떠한 WRITE도 금지 (Control Plane Read-Only Invariant)
- Target Repository 외 타 레포지토리 및 `repository_root` 바깥 파일 수정 절대 금지
- `docker system prune` 등 파괴적 shared infrastructure 명령 사용 금지
- Git Tag는 사용자의 명시적 확정 지시가 있을 때만 생성
