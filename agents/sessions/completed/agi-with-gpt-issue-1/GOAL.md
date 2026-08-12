# 🎯 GOAL.md — agi-with-gpt-issue-1

## Context & Target

* **Target Repository**: `devcy0922/agi-with-gpt`
* **Repository Root**: `/Users/yooncy/srv/agi-with-gpt`
* **Session ID**: `agi-with-gpt-issue-1`

---

## FINAL GOAL

> ChatGPT 작업 프롬프트 수신 시 Coding Agent가 Target Repo에서 계획 → Second Inspection → 승인 없는 자율 구현 → 테스트 → 배포 → Smoke Test → 문서 갱신 → 세션 아카이브 → 상태 요약 및 다음 Action 3개 추천까지 수행하는 대화 기반 Autonomous Coding Loop POC 구축.

---

## DONE IF (SUCCESS CRITERIA)

* [x] 조건 1: GitHub Scheduler/Polling 의존 보류 및 대화 기반 루프 문서/규칙 확립
* [x] 조건 2: Repository Isolation, Filesystem Boundary, Shared Infra Guardrails 정의 및 반영
* [x] 조건 3: `agents/repositories/<repo-name>/` 4대 장기 문서 (`PROJECT`, `STATE`, `ROADMAP`, `DEPLOYMENT`) 구조 구축
* [x] 조건 4: Second Inspection, 승인 없는 자율 실행, 실배포/Smoke Test, Status 요약 & Next 3 Actions, Git Tag 정책 반영
* [x] 조건 5: Git commit & push 완료 (Git Tag 미생성)

---

## MUST NOT 🚫 (SYSTEM INVARIANTS)

* ❌ `.env` 및 시크릿/사설 IP 절대 노출 금지
* ❌ Target Repository 외 타 레포지토리 파일 수정 금지
* ❌ Repository Root 바깥 파일 수정 금지
* ❌ Shared Infrastructure 파괴적 명령 금지
* ❌ 승인 없는 임의 Git Tag 생성 금지
