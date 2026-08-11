# GoVail Agents 체계 조사 및 agi-with-gpt SSOT 설계 보고서

본 문서는 기존 GoVail 레포지토리의 `agents/` 체계를 분석하고, 이를 바탕으로 `agi-with-gpt` 레포지토리의 Agent 운영 단일 진실의 출처(SSOT)를 구축하기 위한 조사 및 판정 보고서입니다.

---

## 1. GoVail agents 체계 분석 (Lifecycle & Usage)

기존 GoVail 레포지토리의 `agents/`는 LLM Agent 작업의 일관성과 검증 게이트를 강제하기 위해 설계되었습니다.

### 1-1. 구성 요소별 발견 & 실행 추적

1. **Rule (`agents/CORE_RULES.md` & `agents/rules/`)**
   - **발견 & 학습**: 에이전트는 작업 진입 시 최우선으로 `agents/rules.md` 및 `CORE_RULES.md`를 읽어 컨텍스트를 초기화합니다.
   - **역할**: 언어 규정(한국어), 노드 경계(`m1-max`, `macmini`, `dgx-spark`), 실빌드/실기동/실검증 3단계 완료 게이트, 보안 6대 원칙을 강제합니다.

2. **Skill (`agents/skills/{skill-name}/SKILL.md`)**
   - **선택 & 적용**: 도메인 특화(예: `govail-gateway`, `govail-runtime`, `design-system`) 작업 시 해당 스킬 문서를 읽어 체크리스트와 도메인 규칙을 준수합니다.
   - **수명**: 여러 세션에 걸쳐 지속적으로 재사용되는 비즈니스/개발 절차입니다.

3. **Template (`agents/templates/`)**
   - **사용법**: 세션 생성 시 `GOAL.template.md`, `PLAN.template.md`, `session-state.template.json`을 복사하여 `agents/sessions/active/{work-id}/` 하위에 세션 파일들을 생성합니다.
   - **역할**: GOAL(목적/제약), PLAN(단계별 실행/빌드/검증 계획), State(JSON 영속 상태)의 표준 형식을 보장합니다.

4. **Workflow (`agents/workflows/packs/`)**
   - **제한 & 전환**: 작업 단계(Planning ➔ Coding ➔ Verification ➔ Review) 및 State transition을 정의하여 작업 상태 변경 규칙을 가이드합니다.

5. **Session (`agents/sessions/active/` & `agents/sessions/archive/`)**
   - **생성 & 진행**: 작업 시작 시 고유 `work-id` 세션 디렉토리를 생성하고 `state.json`을 `IN_PROGRESS`로 기록합니다.
   - **영속화**: 각 작업 턴마다 `state.json`에 timestamp, decisionLog, modifiedFiles, handoffs 등을 업데이트하여 대화 맥락이 끊겨도 상태를 복원합니다.
   - **완료 게이트**: 최종 E2E 검증 후 `state.json`을 `DONE`으로 변경하고 `agents/sessions/archive/`로 이동합니다.

---

## 2. agi-with-gpt용 agents SSOT 구축 판정 (KEEP / ADAPT / REMOVE / ADD)

| 판정 | 대상 항목 | 개요 및 판정 근거 |
| :--- | :--- | :--- |
| **KEEP** | **기본 디렉토리 구조** (`rules`, `skills`, `templates`, `workflows`, `sessions`) | GoVail에서 검증된 5대 핵심 축(행동/능력/양식/순서/실행상태)의 직관적 구분을 그대로 유지 |
| **KEEP** | **GOAL / PLAN / State 삼각 축 양식** | GOAL.md(목적/제약), PLAN.md(단계/검증), state.json(기계 가독형 상태) 영속화 패턴 유지 |
| **ADAPT** | **Session 디렉터리 및 위치** | 별도의 `.ai/state` 없이 `agents/sessions/active/<session-id>/`와 `agents/sessions/completed/<session-id>/`로 일원화 |
| **ADAPT** | **Session ID 명명 규칙** | 기존의 단일 모노레포용 `work-id` 대신, 다중 레포/다중 이슈 지원을 위해 `<target-repo>-issue-<github-number>` (예: `promptia-issue-31`) 양식 적용 |
| **ADAPT** | **State Schema (`state.json`)** | 멀티 에이전트 Collaboration Loop를 지원하도록 `targetRepo`, `githubIssueNumber`, `githubIssueUrl`, `prUrl`, `reviewStatus` (`WAITING_FOR_PLAN_REVIEW`, `APPROVED_FOR_NEXT_STAGE`, `CHANGES_REQUESTED`, `WAITING_FOR_PR_REVIEW` 등) 필드 추가 |
| **REMOVE** | **GoVail 전용 스킬/앱** | `govail-gateway`, `govail-dlp-policy`, `govail-router` 등 GoVail 모노레포 전용 스킬 및 `apps/`, `packages/` 연동 규칙 제거 |
| **REMOVE** | **미구현 오케스트레이션 데몬** | 초기 V1에서는 불필요한 자동화 데몬/파이프라인 코드를 배제하고 명확한 문맥 규칙과 GitHub Interface 기반으로 동작 |
| **ADD** | **Collaboration Loop Workflow** | ChatGPT (기획/이슈/리뷰/PR승인) ↔ Gemini (Primary Worker) ↔ Local LLM (Secondary Worker) 간 피드백 순환 워크플로우 문서 추가 |
| **ADD** | **보안 & 격리 규칙** | `.env` 커밋 금지, 사설 IP (`192.168.x.x`) 및 API Key 유출 방지 검증 수칙 명시 |

---

## 3. 결론

agi-with-gpt의 `agents/`는 여러 서브 프로젝트(Promptia, PDF 분석기 등)를 넘나들며 에이전트들이 일관된 상태와 표준 프로세스로 개발을 수행할 수 있도록 지원하는 **Single Source of Truth(SSOT)**로서 작동합니다.
