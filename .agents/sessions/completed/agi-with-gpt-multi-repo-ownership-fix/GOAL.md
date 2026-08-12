# GOAL — Multi-Repository State Ownership Boundary Fix

## Target Repository
- Name: `devcy0922/agi-with-gpt`
- Root: `/Users/yooncy/srv/agi-with-gpt`

## Primary Goal
Control Repository(`agi-with-gpt`)와 Target Repository간의 State Ownership 및 Isolation Boundary 모순을 해결하는 아키텍처 개편.
Target Repository 작업 시 Control Repository(`agi-with-gpt`)에 어떠한 Write 작업도 발생하지 않고, 모든 가변 상태(4대 문서, Active/Completed Sessions)가 Target Repository의 `.agents/` 디렉토리 내에만 존재하도록 규정 및 가드레일 강제.

## Done Criteria
1. `AGENTS.md` 및 `agents/rules/` (`00-core-rules.md` ~ `04-execution-and-deployment.md`) 갱신하여 Control vs Target Repository 역할 분리 명시.
2. `agents/workflows/` (`collaboration-loop.md`, `session-transition.md`, 필요시 `bootstrap.md`) 갱신.
3. 중앙 `agents/repositories/` 및 `agents/sessions/` 제거하고, `agi-with-gpt`의 자사 상태를 `.agents/` 하위로 완전 이관.
4. `.agents` 자동 Bootstrap 절차 규정 및 템플릿 참조 가이드 완성.
5. Boundary Acceptance Test 정적 검증 수행 (Write Path 제약 및 Enforcement Level 명시).
6. Commit & Push 완료 (Git Tag 미생성).
7. 최종 결과 보고서 작성 (Next Action 1번: GoVail E2E 실증).
