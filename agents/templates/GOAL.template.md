# 🎯 GOAL.md — {session-id}

<!-- ⚠️ 작성 지침:
  - 300 토큰 이하로 간결하게 작성
  - 구현 디테일(특정 라이브러리, 구현 방법) 금지, "무엇"만 정의
  - 모든 Done 조건은 YES/NO 판단 가능해야 함
-->

## Context & Target

* **Target Repository**: `{target-repo}`
* **Repository Root**: `{repository-root}`
* **Session ID**: `{session-id}`

---

## FINAL GOAL

> 이 세션의 최종 상태 및 핵심 목적

* [ ] 무엇을 완성해야 하는가?

---

## DONE IF (SUCCESS CRITERIA)

> "완료"를 판단하는 명확한 조건 (YES/NO 판단 가능)

* [ ] 조건 1: 코드 구현 및 빌드/테스트 통과
* [ ] 조건 2: 실배포 및 배포 후 Smoke Test 검증 완료
* [ ] 조건 3: STATE.md 및 ROADMAP.md 문서 갱신 완료

---

## MUST NOT 🚫 (SYSTEM INVARIANTS)

> 절대 위반 금지 (위반 시 무조건 FAIL 또는 BLOCKED)

* ❌ `.env` 및 시크릿/사설 IP 절대 노출 금지
* ❌ Target Repository 이외 타 Repository 파일 수정 금지
* ❌ Repository Root (`{repository-root}`) 바깥 파일 수정 금지
* ❌ Shared Infrastructure (`docker system prune` 등) 파괴적 조작 금지

---

## SCOPE

### IN SCOPE
* {포함할 작업}

### OUT OF SCOPE
* {제외할 작업}

