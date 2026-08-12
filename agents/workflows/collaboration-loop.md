# collaboration-loop.md — 대화 기반 Autonomous Coding Loop 워크플로우

본 워크플로우는 ChatGPT, User(Human Gate), Coding Agent(Gemini Primary Worker / Secondary Worker) 간의 프롬프트 복붙 기반 자율 순환 개발 프로세스입니다.

---

## 1. 전체 Autonomous Coding Loop 다이어그램

```text
ChatGPT
    │
    │ 1. 결과 검토 & 다음 Prompt 생성
    ▼
User (Human Gate)
    │
    │ 2. Prompt 복사 후 Coding Agent에 입력
    ▼
Coding Agent
    │
    ├─ 3. Target Repository 확인 및 Root / Boundary 검증
    ├─ 4. Repository 분석 (PROJECT, STATE, ROADMAP, DEPLOYMENT, Current Code)
    ├─ 5. Active Session 생성 또는 기존 Session 재개
    ├─ 6. PLAN 작성
    ├─ 7. 코드 재검토 (Second Inspection) & PLAN Final 확정
    ├─ 8. 승인 없이 자율 구현 (Implementation)
    ├─ 9. 자동 빌드 & 단위/통합 테스트 (Test & Build)
    ├─ 10. 실배포 (Deployment)
    ├─ 11. 배포 후 검증 (Post-Deploy Smoke Test)
    ├─ 12. 레포지토리 문서 업데이트 (STATE.md / ROADMAP.md 갱신)
    ├─ 13. 세션 아카이브 (completed 이동)
    ├─ 14. 현재 상태 요약 (Current Status)
    └─ 15. 다음 Action 정확히 3개 추천 (Recommended Next Actions)
    │
    ▼
User (Human Gate)
    │
    │ 16. 결과 직접 검증 및 ChatGPT에 결과 전달
    ▼
ChatGPT
    │
    └─ (다음 작업 Prompt 작성 후 반복)
```

---

## 2. 역할별 핵심 행동 지침 (Role Responsibilities)

### 1) ChatGPT (Planner, Reviewer, Next Action Selector)
- **결과 검토**: User가 전달한 실행 결과, Current Status, 배포/테스트 로그 검토.
- **다음 작업 판단**: 로드맵 및 현 상태 기반으로 추진할 최적의 다음 작업 결정.
- **실행 Prompt 작성**: `target_repository`, `repository_root`, 구체적 `Goal`이 포함된 Prompt 생성.

### 2) Coding Agent (Primary Worker — Gemini)
- **Repository Isolation 준수**: Target Repo 외 타 Repo, Repo Root 밖 파일, Shared Infra 절대 수정 금지.
- **Plan & Second Inspection**: Plan 작성 후 실제 소스코드를 재검토하여 PLAN Final 확정.
- **자율 실행 (Approval-Free)**: 중간 승인 대기 없이 구현 → 빌드 → 테스트 → 배포 → Smoke Test 자율 완료.
- **문서 갱신 & 아카이브**: `STATE.md`, `ROADMAP.md` 갱신 및 완료 세션을 `completed`로 이관.
- **결과 요약 및 추천**: `Current Status` 요약 및 `Recommended Next Actions` 3개 추천.

### 3) User (Human Gate)
- 작업 실행 중간 승인 요청 없이 **한 작업 사이클이 끝난 시점**에만 개입.
- 결과를 직접 확인하고, 해당 요약과 로그를 ChatGPT에 전달하여 다음 Prompt 수신.
- 사용자 명시적 승인시에만 Git Tag 생성 허용.

---

## 3. GitHub & Session의 데이터 분리 원칙 (Reference-Only)

- **GitHub**: 사람과 Agent가 소통하는 **Collaboration Interface** (Issue, Comment, Commit, PR, Approval).
- **agents/sessions/**: Agent가 작업을 영속적으로 이어가기 위한 **Execution State** (GOAL, PLAN, state.json, 결정 로그, 수정 파일).
- **분리 원칙**: GitHub 코멘트 전문을 Session 문서에 그대로 복사하거나, Session의 전 내용을 Issue body에 복사하지 않으며, **상호 Reference 경로(URL 및 Session ID)**만 연결합니다.
