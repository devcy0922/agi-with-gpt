# session-transition.md — Session 상태 및 아카이브 워크플로우

본 워크플로우는 `state.json`의 상태 전환 및 활성 세션의 완료 처리 규칙을 정의합니다.

---

## 1. State Transition Matrix

| Current Stage | Event / Trigger | Target Stage | Review Status | Next Action |
| :--- | :--- | :--- | :--- | :--- |
| `INIT` | Session 디렉토리 및 GOAL/PLAN 작성 완료 | `PLANNING` | `WAITING_FOR_PLAN_REVIEW` | Issue Comment 작성 후 리뷰 대기 |
| `PLANNING` | ChatGPT: `CHANGES_REQUESTED` | `PLANNING` | `PLAN_REVISION_REQUIRED` | Worker: GOAL/PLAN 수정 및 Re-comment |
| `PLANNING` | ChatGPT: `APPROVED_FOR_NEXT_STAGE` | `CODING` | `PLAN_APPROVED` | Worker: 코드 구현 시작 |
| `CODING` | 코드 수정 및 로컬 테스트/빌드 완료 | `VERIFYING` | `IN_VERIFICATION` | Worker: 런타임/통합 검증 실행 |
| `VERIFYING` | 실검증 통과 및 PR 생성 완료 | `PR_SUBMITTED` | `WAITING_FOR_PR_REVIEW` | ChatGPT: PR 검토 대기 |
| `PR_SUBMITTED` | ChatGPT: `REQUEST_CHANGES` | `CODING` | `PR_REVISION_REQUIRED` | Worker: 코드 수정 및 PR 갱신 |
| `PR_SUBMITTED` | ChatGPT: `APPROVED` & Merged | `DONE` | `COMPLETED` | Session 디렉토리 completed 이동 |

---

## 2. Session Complete (Archive) 절차

작업이 승인되고 머지되어 완료되면:

1. `state.json` 내 `status`를 `"DONE"`, `currentStage`를 `"DONE"`, `updatedAt`을 현재 시각으로 설정합니다.
2. 실행 상태 디렉토리를 `active`에서 `completed`로 이동합니다:
   ```bash
   mv agents/sessions/active/<session-id> agents/sessions/completed/
   ```
3. git commit을 남겨 완료 세션 기록을 보존합니다.
