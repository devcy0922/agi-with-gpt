# session-transition.md — Session 상태 전환 및 아카이브 워크플로우

본 워크플로우는 `state.json`의 수명주기 상태 전환 Matrix 및 완료 세션의 아카이브 절차를 정의합니다.

---

## 1. State Transition Matrix

| Current Stage | Event / Trigger | Target Stage | Next Action / Behavior |
| :--- | :--- | :--- | :--- |
| `CREATED` | Prompt 수신 및 Target Repo 검증 완료 | `REPOSITORY_ANALYSIS` | 레포 4대 문서 및 소스코드 분석 |
| `REPOSITORY_ANALYSIS` | 레포지토리 분석 완료 | `PLANNING` | `GOAL.md`, `PLAN.md` (v1) 작성 |
| `PLANNING` | PLAN v1 작성 완료 | `PLAN_VALIDATION` | 소스코드 재검토 (**Second Inspection**) 및 PLAN Final 확정 |
| `PLAN_VALIDATION` | PLAN Final 확정 완료 | `IMPLEMENTING` | Target Repo 소스코드 구현 시작 (승인 대기 없음) |
| `IMPLEMENTING` | 코드 구현 완료 | `TESTING` | 단위 테스트 및 빌드 검증 실행 |
| `TESTING` | 빌드/테스트 통과 완료 | `DEPLOYING` | Target Repo `DEPLOYMENT.md` 방식대로 실배포 |
| `DEPLOYING` | 배포 완료 | `POST_DEPLOY_VALIDATION` | Smoke Test 및 런타임/로그 검증 |
| `POST_DEPLOY_VALIDATION`| Smoke Test 성공 | `DOCUMENTING` | `STATE.md`, `ROADMAP.md` 등 레포 문서 업데이트 |
| `DOCUMENTING` | 문서 업데이트 완료 | `COMPLETED` | 세션 디렉토리를 `completed/`로 이동 |
| `COMPLETED` | 아카이브 완료 | `ARCHIVED` | `Current Status` 및 `Recommended Next Actions` (3개) 보고 후 세션 종료 |

### 예외 Transition:
- 실행 불가 위험 요소(Repo Root 바깥 수정, 타 Repo 변경, Shared Infra 조작, Secret 필요 등) 발견 시 → `BLOCKED_REQUIRES_USER_DECISION`
- 해결 불가 에러 발생 시 → `BLOCKED` 또는 `FAILED`

---

## 2. Session Complete (Archive) 절차

작업이 완료되면:

1. `state.json` 내 `status`를 `"COMPLETED"`, `currentStage`를 `"COMPLETED"`로 변경합니다.
2. 실행 상태 디렉토리를 `active`에서 `completed`로 이동합니다:
   ```bash
   mv agents/sessions/active/<session-id> agents/sessions/completed/
   ```
3. Git commit을 수행하여 아카이브 세션을 저장합니다.

