# 04-execution-and-deployment.md — 자율 실행 및 배포/검증 규정

> **MANDATORY**: 본 규칙은 작업 시작부터 PLAN 작성, Second Inspection, 승인 없는 자율 실행, 실배포, 배포 후 Smoke Test, 문서 갱신, 세션 아카이브, 상태 요약 및 다음 Action 3개 제안까지의 전체 자율 실행 루프를 정의합니다.

---

## 1. 작업 시작 절차 (Intake Procedure — 12 Steps)

프롬프트를 수신하면 바로 코드를 수정하지 않고 다음 12단계를 순서대로 수행합니다.

1. **Target Repository 확인**
2. **Repository Root 검증** (Filesystem & Repo Boundary 확인)
3. **git status 확인**
4. **현재 branch 확인**
5. **최근 commit 확인** (`git log -n 5`)
6. **`<target-repository>/.agents/` 유효성 확인** (없을 시 Bootstrap 실행)
7. **Target `.agents/PROJECT.md` 및 `DEPLOYMENT.md` 확인**
8. **Target `.agents/STATE.md` 및 `ROADMAP.md` 확인**
9. **실제 코드 구조 확인**
10. **요청 분석**
11. **Session 생성 또는 기존 Active Session 재개** (`<target-repository>/.agents/sessions/`)
12. **PLAN 작성**

---

## 2. PLAN 작성 및 Second Inspection (자기 검수)

### 1) PLAN 필수 항목:
- Goal
- Current State
- Scope
- Files likely to change
- Implementation strategy
- Validation strategy
- Deployment strategy
- Risks & Boundaries
- Out of Scope

### 2) PLAN 작성 후 Second Inspection (필수 2차 검수):
PLAN v1을 작성한 직후 바로 구현에 들어가지 않고, 실제 코드베이스를 재검토하여 PLAN을 검증/수정합니다.

#### Second Inspection 체크리스트:
- [ ] 이미 구현된 기능은 없는가?
- [ ] PLAN에서 잘못 이해한 코드가 없는가?
- [ ] 수정 예정 파일 범위가 적절한가?
- [ ] 더 작은 변경으로 해결 가능한가?
- [ ] 중복 기능을 만들고 있지 않은가?
- [ ] 현재 Architecture와 충돌하지 않는가?
- [ ] 테스트 범위가 충분한가?
- [ ] 배포 방식이 실제 환경(`DEPLOYMENT.md`)과 일치하는가?
- [ ] Repository Boundary 및 Filesystem Boundary를 침범하지 않는가?
- [ ] **모든 Write Path가 Target Repository 루트(`repository_root/**`) 내부인가?** (Control Repository `agi-with-gpt` 수정 시도 포함 차단)

검토 결과 문제가 발견되면 PLAN을 수정하여 **FINAL PLAN**으로 확정합니다.

---

## 3. 승인 없는 자율 구현 실행 Loop (Auto Execution)

FINAL PLAN 확정 후 ChatGPT나 사용자의 중간 승인 없이 아래 단계를 자율 수행합니다.

```text
IMPLEMENTING
   ↓
TESTING
   ↓
BUILD
   ↓
INTEGRATION TEST
   ↓
COMMIT & PUSH
   ↓
DEPLOY
   ↓
POST_DEPLOY_VALIDATION (Smoke Test)
   ↓
DOCUMENTATION (STATE / ROADMAP Update inside Target .agents/)
   ↓
SESSION COMPLETED & ARCHIVED
```

---

## 4. 완료 정의 & 실배포 & Smoke Test

### 1) 완료 정의 (Definition of Done):
`Implementation + Automated Test + Build + Deploy + Runtime Verification (Smoke Test) + Documentation Update + Session Archive`가 모두 이루어져야 완료입니다. (단순 코드 수정이나 unit test만으로는 완료가 아님)

### 2) 실배포 (Production Deployment):
- 대상 레포지토리의 `DEPLOYMENT.md`에 명시된 방식을 조사하여 준수합니다. (Docker Compose, Mac Mini local service, GCP VM, Cloud Run, Vercel 등)
- 배포 환경을 추측해서 임의로 변경하지 않습니다.

### 3) 배포 후 테스트 (Smoke Test):
배포 성공 로그만 보고 끝내지 않으며 actual Smoke Test를 수행합니다:
- Health endpoint 호출 (`curl` / API request)
- Container health & Application logs 확인
- 실제 주요 사용자 flow 또는 Frontend 렌더링 확인

### 4) 실패 시 처리 정책:
- 테스트 또는 배포 실패 시 Reasonable 범위 내에서 `원인 분석 → 수정 → 재테스트 → 재배포 → 재검증`을 수행합니다.
- 동일 실패를 무한 반복하지 않으며, 구조적 blocker 확인 시 `BLOCKED` 상태로 전환하고 원인을 상세히 기록합니다.

---

## 5. 완료 문서 갱신 & Session Archive

작업 완료 시 반드시 Target Repository의 레포지토리 4대 문서(`<target-repository>/.agents/STATE.md`, `ROADMAP.md`, 필요시 `PROJECT.md`, `DEPLOYMENT.md`)를 실제 결과에 맞춰 업데이트합니다.
완료된 세션은 `<target-repository>/.agents/sessions/active/<session-id>`에서 `<target-repository>/.agents/sessions/completed/<session-id>`로 이동(Archive)합니다.

---

## 6. Current Status 요약 및 Recommended Next Actions (정확히 3개)

모든 작업 완료 및 아카이브 후 최종 출력 형식:

```text
## Current Status

Project Phase:
Current Production State:
Test State:
Deployment State:
Major Completed Work:
Remaining Problems:
Technical Debt:

## Recommended Next Actions

### 1. <작업 1 명칭>
Why:
Expected Impact:
Risk:
Scope:

### 2. <작업 2 명칭>
Why:
Expected Impact:
Risk:
Scope:

### 3. <작업 3 명칭>
Why:
Expected Impact:
Risk:
Scope:
```

### 추천 기준:
단순 ROADMAP 순서가 아니라 프로젝트 최종 목표, 현재 아키텍처, blocker, 기술부채, 운영 안정성, 사용자 가치, 사전 조건(prerequisite)을 종합 고려하여 **우선순위 순으로 정확히 3개** 제안합니다.

---

## 7. Git Tag 정책 (User-Confirmed Tagging)

Git Tag는 Coding Agent가 자율적 Loop 동안 임의로 생성하지 않습니다.
오직 사용자가 명시적으로 다음과 같이 확정했을 때만 실행합니다:
- *"이 버전을 확정한다."*
- *"태그 찍자."*
- *"v0.x로 확정."*

승인이 확인된 경우에만 `git status`, `HEAD`, 테스트/배포 상태 확인 후 Git Tag를 생성하고 push합니다.

---

## 8. Guardrail Enforcement 수준 명시 가이드

문서 규칙(Policy)과 실제 기술적/자동화적 Enforcement 수준을 엄격하게 구분하여 보고합니다. 검증되지 않은 항목에 대해 절대적 표현(`Remaining Problems: None`, `Technical Debt: None`, `E2E_VERIFIED`)을 근거 없이 사용하는 것을 금지합니다.

### Enforcement Levels:
- **`POLICY_DEFINED`**: 규칙/문서상에 원칙이 명시되어 있는 상태.
- **`DETERMINISTIC_CHECK_AVAILABLE`**: 정적 검사 스크립트나 경로 체크 알고리즘으로 검증 가능한 상태.
- **`RUNTIME_ENFORCED`**: Agent 실행 루프나 도구 가드레일 레벨에서 실시간으로 Write Path / Shell 커맨드가 차단되는 상태.
- **`E2E_VERIFIED`**: 실제 Target Repository 환경에서 전체 자율 루프(계획-구현-배포-검증-아카이브)가 작동 실증된 상태.

