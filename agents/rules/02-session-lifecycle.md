# 02-session-lifecycle.md — Session 수명주기 및 실행 상태 관리 규정

> **MANDATORY**: 모든 작업의 실행 상태(Execution State)는 Session 디렉토리를 SSOT로 관리합니다.

---

## 1. Session ID 명명 규칙 & Session 재사용 (Continuation)

### 1) ID 명명 규칙:
```text
<target-repo>-issue-<github-number> (또는 <target-repo>-session-<timestamp/task-name>)
```
예시:
- `govail-issue-52`
- `agi-with-gpt-issue-1`
- `govail-session-router-test`

### 2) Session Continuation 규칙:
모든 Prompt마다 무조건 새로운 Session을 만들지 않습니다.
기존 Active Session과:
1. **Target Repository 동일**
2. **Goal 동일**
3. **기존 작업의 직접적인 연속**

인 경우 기존 Active Session을 재사용하여 상태를 이어갑니다. 새로운 독립 목표인 경우에만 새 Session을 생성합니다.

---

## 2. Session 위치 및 파일 구조

모든 세션 상태는 **Target Repository 내부**의 `.agents/sessions/` 디렉토리에만 보관됩니다. (Control Repository `agi-with-gpt`에 저장하지 않습니다.)

### 활성 상태 (Active):
`<target-repository>/.agents/sessions/active/<session-id>/`

### 완료 상태 (Completed):
`<target-repository>/.agents/sessions/completed/<session-id>/`

### 필수 3대 파일 구성:
1. `GOAL.md` — Target Repo, Root, 최종 목적, Done 조건 (양식: `agi-with-gpt/agents/templates/GOAL.template.md`)
2. `PLAN.md` — 실행 계획, Second Inspection 검증, 테스트/배포/Smoke Test 전략 (양식: `agi-with-gpt/agents/templates/PLAN.template.md`)
3. `state.json` — 실시간 machine-readable 상태 (양식: `agi-with-gpt/agents/templates/session-state.template.json`)

---

## 3. POC 단순화 State Machine (Approval-Free Execution)

POC 단계에서는 매 단계 ChatGPT 승인 대기(`WAITING_FOR_CHATGPT_REVIEW`, `WAITING_FOR_PLAN_REVIEW`)를 제거하고 **PLAN 작성 및 Second Inspection 완료 후 즉시 자율 구현**합니다.

```text
CREATED
   ↓
REPOSITORY_ANALYSIS
   ↓
PLANNING
   ↓
PLAN_VALIDATION (Second Inspection)
   ↓
IMPLEMENTING
   ↓
TESTING
   ↓
DEPLOYING
   ↓
POST_DEPLOY_VALIDATION
   ↓
DOCUMENTING
   ↓
COMPLETED
   ↓
ARCHIVED
```

### 예외/중단 상태:
- `BLOCKED`: 일반적 해결 불가 blocker 발생 시 원인 기록 후 대기
- `FAILED`: 반복적 테스트/배포 실패 시
- `BLOCKED_REQUIRES_USER_DECISION`:
  다음 조건에 해당하는 경우 실행을 중지하고 사용자 결정을 대기합니다.
  - Repository Root 바깥 파일 수정 필요 시
  - 타 Repository 변경 필요 시 (Control Repository `agi-with-gpt` 수정 요구 포함)
  - Shared Infrastructure 변경 필요 시
  - Credential / Secret 필요 시
  - 파괴적인 DB Migration 필요 시
  - Production Data 손실 가능성 시

---

## 4. Session Archive 절차

작업이 완료되면:

1. `state.json`의 `status`를 `"COMPLETED"`, `currentStage`를 `"COMPLETED"`로 변경합니다.
2. 세션 문서에 최소 다음 기록이 보존되어 있는지 최종 검증합니다:
   - `GOAL`
   - `FINAL PLAN`
   - 실제 변경 내용 및 파일 목록
   - 테스트 결과 & 빌드 결과
   - 배포 결과 & Smoke Test 결과
   - Commit SHA 및 timestamp
   - 남은 문제 / 기술부채
3. Target Repository 내부에서 세션 디렉토리를 `active`에서 `completed`로 이동합니다:
   ```bash
   mv <target-repository>/.agents/sessions/active/<session-id> <target-repository>/.agents/sessions/completed/
   ```
4. git commit을 남겨 Target Repository 내에 아카이브 세션을 보존합니다.

