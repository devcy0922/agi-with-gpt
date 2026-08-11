# collaboration-loop.md — ChatGPT & Coding Agent 협업 워크플로우

본 워크플로우는 ChatGPT, Gemini(Primary Worker), Local LLM(Secondary Worker) 간의 깃허브 및 세션 기반 순환 개발 프로세스입니다.

---

## 1. 전체 Collaboration Loop 다이어그램

```text
ChatGPT
    │
    │ 1. 다음 작업 판단 & 기획
    ▼
GitHub Issue 생성
    │
    ▼
Gemini / Local LLM (Worker)
    │
    ├─ 2. Issue 분석 & Target Repo 조사
    │
    ▼
agents/sessions/active/<session-id>/
    ├─ GOAL.md
    ├─ PLAN.md
    └─ state.json
    │
    ▼
GitHub Issue Comment
"Plan 준비 완료 / Session 경로"
    │
    ▼
ChatGPT Review
    │
    ├─ APPROVED_FOR_NEXT_STAGE ───┐
    │                            │
    └─ CHANGES_REQUESTED         │
             │                   │
             ▼                   │
        Worker 수정              │
             │                   │
             └───────────────────┘
                                 │
                                 ▼
                           Implementation
                                 │
                                 ▼
                      Test / Build / Verify
                                 │
                                 ▼
                          ChatGPT Review
                                 │
                                 ▼
                              PR 생성
                                 │
                                 ▼
                          ChatGPT PR Review
                           ├─ APPROVE
                           └─ REQUEST_CHANGES
                                 │
                                 ▼
                             Worker 수정
                                 │
                                 └───── 반복 (승인 시 Complete)
```

---

## 2. 역할별 핵심 행동 지칙 (Role Responsibilities)

### 1) ChatGPT (Planner & Reviewer)
- **기획 & 이슈 작성**: Target Repository의 필요한 작업 및 버그를 정의하여 GitHub Issue 생성.
- **PLAN / GOAL Review**: Worker가 생성한 세션 문서(`GOAL.md`, `PLAN.md`)를 검토하고 `APPROVED_FOR_NEXT_STAGE` 또는 `CHANGES_REQUESTED` 의견 전달.
- **PR Review & Approval**: Worker가 작성한 PR 및 검증 로그 확인 후 최종 승인(Approve & Merge) 수행.

### 2) Gemini (Primary Coding Worker)
- **이슈 분석 및 조사**: Target Repo를 조사하고, `agents/sessions/active/<session-id>/` 하위에 세션 파일 생성.
- **이슈 코멘트 남기기**: 세션 경로 및 계획 요약 작성 후 리뷰 요청.
- **구현 & 검증**: Plan 승인 완료 시 실제 코드 작성, 빌드 및 테스트 검증 수행.
- **PR 생성**: 변경 사항과 실검증 결과 로그를 포함하여 PR 작성.

### 3) Local LLM (Secondary Coding Worker)
- 반복적 코드 작업, 리팩토링 보조, 테스트 코드 작성, 정적 분석 지원.

---

## 3. GitHub & Session의 데이터 분리 원칙 (Reference-Only)

- **GitHub**: 사람과 Agent가 소통하는 **Collaboration Interface** (Issue, Comment, Commit, PR, Approval).
- **agents/sessions/**: Agent가 작업을 영속적으로 이어가기 위한 **Execution State** (GOAL, PLAN, state.json, 결정 로그, 수정 파일).
- **분리 원칙**: GitHub 코멘트 전문을 Session 문서에 그대로 복사하거나, Session의 전 내용을 Issue body에 복사하지 않으며, **상호 Reference 경로(URL 및 Session ID)**만 연결합니다.
