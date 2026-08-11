# 02-session-lifecycle.md — Session 실행 상태 관리 규정

> **MANDATORY**: 모든 작업의 실행 상태(Execution State)는 Session 디렉토리를 SSOT로 사용합니다.
> 별도의 `.ai/state` 디렉토리를 생성하지 않으며, Session이 유일한 실시간 영속화 위치입니다.

---

## 1. Session ID 명명 규칙

여러 Target Repository의 이슈를 동시 처리하므로 단순 숫자/단어 대신 다음 형식을 엄격히 적용합니다.

### 형식:
```text
<target-repo>-issue-<github-number>
```

### 예시:
- `promptia-issue-31`
- `document-validator-issue-7`
- `govail-issue-52`

---

## 2. Session 위치 및 파일 구조

### 활성 상태 (Active):
`agents/sessions/active/<session-id>/`

### 완료 상태 (Completed):
`agents/sessions/completed/<session-id>/`

### 필수 3대 파일 구성:
1. `GOAL.md` — 작업 목적, Done 조건, Hard Constraints (양식: `agents/templates/GOAL.template.md`)
2. `PLAN.md` — 단계별 실행 절차, 빌드/검증 명령, 위험 요소 (양식: `agents/templates/PLAN.template.md`)
3. `state.json` — 실시간 machine-readable 상태, 타임라인, 수정 파일 목록 (양식: `agents/templates/session-state.template.json`)

---

## 3. GitHub Push 의무 & 리뷰 요청 마커 (MANDATORY)

> **CRITICAL**: 리뷰어를 포함한 사용자는 GitHub상에 올라온 상태만 볼 수 있습니다. 
> 로컬 `~/srv/agi-with-gpt`에만 세션 문서가 존재하면 리뷰할 수 없습니다.

### 1) Git Push 필수 규정:
Worker Agent가 리뷰를 요청할 때는 **반드시** 세션 디렉토리(`agents/sessions/active/<session-id>/`) 및 작업 브랜치를 GitHub remote에 `git push`한 후 코멘트를 등록해야 합니다.

### 2) 표준 리뷰 마커 (Standard Review Markers):
- **Plan / 중간 구현 리뷰 요청 시**: `[READY_FOR_REVIEW]`
- **최종 PR 리뷰 요청 시**: `[READY_FOR_PR_REVIEW]`

### 3) Issue 코멘트 양식:
```text
[READY_FOR_REVIEW]

Session:
agents/sessions/active/<session-id>/

Branch:
agent/<session-id>

Review cycle:
1
```

---

## 4. Session 수명주기 단계 (Lifecycle Stages)

1. **Issue 수신 & Session 생성**:
   - Target Repo 및 GitHub Issue 번호를 파악하여 Session ID 결정.
   - `agents/templates/`의 양식을 `agents/sessions/active/<session-id>/`로 복사.
   - `GOAL.md`, `PLAN.md`, `state.json` 작성 및 `state.json` 상태를 `IN_PROGRESS`로 설정.
2. **Review 요청 & Git Push**:
   - 세션 파일 커밋 후 GitHub `git push`.
   - Issue 코멘트에 `[READY_FOR_REVIEW]` 마커 및 Session 경로, Review cycle 표기.
3. **Review Loop**:
   - ChatGPT Review 결과(`APPROVED_FOR_NEXT_STAGE` / `CHANGES_REQUESTED`) 확인.
   - 이미 처리된 `Review cycle`은 재처리하지 않으며, `CHANGES_REQUESTED` 시 `Review cycle`을 1 증가시키고 세션/코드 수정 후 다시 Push 및 `[READY_FOR_REVIEW]` 코멘트 전송.
4. **Execution & Verification**:
   - Target Repo에서 코드 변경 및 테스트/빌드 검증 수행. `state.json` 내 `decisionLog`, `modifiedFiles` 업데이트.
5. **PR 생성 & PR Review**:
   - PR 생성 후 `[READY_FOR_PR_REVIEW]` 마커와 함께 PR 제출.
6. **Completion & Archive**:
   - PR 승인(`APPROVE` & Merge) 확인 후 `state.json`의 `status`를 `DONE`으로 변경.
   - 세션 디렉토리를 `agents/sessions/completed/<session-id>/`로 이동 후 Git Push.
