# PLAN v1 — Multi-Repository State Ownership Boundary Fix

## Goal
Control Repository (`agi-with-gpt`)와 Target Repository (`govail`, `promptia`, `agi-with-gpt` 등) 간의 State Ownership 경계를 재정립하여, Target 작업 시 Control Repository에 어떠한 Write도 허용하지 않도록 하고, 모든 가변 상태를 Target Repository 루트 내부의 `.agents/` 디렉토리에 전속시키는 아키텍처 개편.

## Current State Analysis
- 현재 `agi-with-gpt/agents/repositories/<repo-name>/` 및 `agi-with-gpt/agents/sessions/`에 타 레포지토리의 문서 및 세션 상태가 위치함.
- Target Repo가 `govail`일 때 가드레일(`repository_root/**` 외 수정 금지)과 완료 시 `agi-with-gpt/agents/...` 수정 요구사항이 정면으로 충돌(Self-Violation).

## Target Architecture
1. **Control Repository (`agi-with-gpt`)**:
   - Immutable/Read-Only Control definitions (`HOW TO WORK`):
     - `agents/rules/`
     - `agents/templates/`
     - `agents/skills/`
     - `agents/workflows/`
   - Target Execution 중에는 Control Repository 내 어떠한 파일도 WRITE 하지 않음 (READ-ONLY).
   - 기존 `agents/repositories/` 및 `agents/sessions/` 제거 (중앙 관리 폐지).

2. **Target Repository (`<target-repository>/`)**:
   - Mutable Project State & Session Execution (`WHAT / CURRENT STATE`):
     - `<target-repository>/.agents/PROJECT.md`
     - `<target-repository>/.agents/STATE.md`
     - `<target-repository>/.agents/ROADMAP.md`
     - `<target-repository>/.agents/DEPLOYMENT.md`
     - `<target-repository>/.agents/sessions/active/<session-id>/`
     - `<target-repository>/.agents/sessions/completed/<session-id>/`

3. **Bootstrap Flow**:
   - Target Repo에 `.agents/`가 없으면 `agi-with-gpt/agents/templates/`를 읽어 Target Repo에 `.agents/` 구조 및 기본 4대 문서를 bootstrap함.
   - 이미 존재하면 기존 상태 보존 및 연속 사용.

4. **Self-Ownership (`agi-with-gpt` 자체)**:
   - `agi-with-gpt` 자체도 Target Repo일 수 있으므로, 자신의 상태/세션은 `.agents/` 하위에 저장.

## Implementation Steps
1. **Rule & Doc Updates**:
   - `AGENTS.md`: `.agents/` 및 Control vs Target Repo Ownership 업데이트.
   - `agents/rules/01-security-and-isolation.md`: Write Path Enforcement 및 Control Repo Read-Only 제약 추가.
   - `agents/rules/02-session-lifecycle.md`: Session 위치를 `<target-repository>/.agents/sessions/`로 변경.
   - `agents/rules/03-repository-management.md`: Repository 4대 문서를 `<target-repository>/.agents/`로 이관하고 중앙 `agents/repositories/` 폐지.
   - `agents/rules/04-execution-and-deployment.md`: Intake, PLAN 작성, Second Inspection, Bootstrap, Archive, Enforcement Level 규정 갱신.
   - `agents/workflows/collaboration-loop.md` 및 `session-transition.md`: `.agents/` 구조 반영 및 Bootstrap 절차 수립.

2. **Directory & File Migration**:
   - `agents/repositories/agi-with-gpt/*` -> `.agents/*` (PROJECT, STATE, ROADMAP, DEPLOYMENT)로 이동 및 갱신.
   - `agents/sessions/completed/agi-with-gpt-issue-1` -> `.agents/sessions/completed/agi-with-gpt-issue-1`로 이동.
   - 기존 중앙 `agents/repositories/` 및 `agents/sessions/` 디렉토리 삭제.

3. **Acceptance Test & Verification**:
   - Boundary Check: Target = `govail` 시 write target이 `/Users/yooncy/srv/govail/**`에 국한되는지 정적 검증.
   - Enforcement Level 명시 (`POLICY_DEFINED`, `DETERMINISTIC_CHECK_AVAILABLE`, `RUNTIME_ENFORCED`, `E2E_VERIFIED`).

4. **Git Commit & Push**:
   - Git Tag 없이 commit 및 remote main push.

## Second Inspection Verification Results
- [x] Write Path가 오직 `<target-repository>/.agents/` 및 소스코드 영역에 국한되는가? -> YES
- [x] Control Repo READ-ONLY 제약이 모든 규칙 문서에 일관되게 반영되었는가? -> YES
- [x] Bootstrap 절차 예시가 명확하게 작성되었는가? -> YES
- [x] 불필요한 중앙 관리 레거시 디렉토리가 모두 정리되었는가? -> YES

**Status**: SECOND_INSPECTION_PASSED -> **FINAL PLAN CONFIRMED**

