# 03-repository-management.md — Target Repository 및 장기 문서 관리 규정

> **MANDATORY**: 모든 작업은 반드시 명시된 Target Repository context 내에서 실행되며, 레포지토리별 4대 장기 문서(`PROJECT.md`, `STATE.md`, `ROADMAP.md`, `DEPLOYMENT.md`)를 SSOT로 유지합니다.

---

## 1. Target Repository 지정 및 검증

작업 Prompt 수신 시 Target Repository 정보를 확인하고 작업 개시 전 검증합니다.

```yaml
target_repository: promptia-create-novel # 실제 Autonomous Coding Target
repository_root: /Users/yooncy/srv/promptia-create-novel
protected_repositories:
  - govail # LLM Infrastructure (수정 금지 Deny-List)
```

### 검증 체크리스트:
1. `repository_root` 경로 존재 여부 및 Git repository 유효성 확인 (`git status`, `git branch`, `git log -n 5`)
2. `repository_root` 바깥 디렉터리로 이탈하지 않는지 확인 (Filesystem Boundary Enforcer)
3. Target Repository가 `protected_repositories` Deny-List에 포함되어 있지 않은지 검증 (GoVail 등 인프라 레포 수정 시도 차단)
4. Target Repository 내부 **`<target-repository>/.agents/`** 유효성 확인 및 4대 문서 읽기

---

## 2. 레포지토리별 장기 문서 구조 (`<target-repository>/.agents/`)

**CRITICAL INVARIANT**: 레포지토리의 장기 문서(`PROJECT.md`, `STATE.md`, `ROADMAP.md`, `DEPLOYMENT.md`)와 세션 상태는 중앙 `agi-with-gpt`가 아닌 **Target Repository 자체**가 소유(SSOT)합니다.

중앙 `agi-with-gpt/agents/repositories/` 구조는 Repository Boundary Violations의 원인이므로 전면 폐지되었습니다.

```text
<target-repository>/
└── .agents/
    ├── PROJECT.md     # 프로젝트 목적, 핵심 아키텍처, 제약 사항
    ├── STATE.md       # 현재 실제 구동/구현 상태, 테스트/배포 상태, Known Issue, Blocker, 기술부채
    ├── ROADMAP.md     # 장기 계획 (Completed, Current, Next, Backlog, Frozen)
    ├── DEPLOYMENT.md  # 배포 환경, 서비스 명, 검증 URL, Smoke Test / Rollback 방법
    └── sessions/
        ├── active/    # 현재 진행 중인 세션
        └── completed/ # 완료 및 아카이브된 세션
```

### 문서별 역할 정의:
- **PROJECT.md**: 프로젝트의 변치 않는 본질과 장기적 정의. (목적, 최종 목표, 아키텍처, 제약)
- **STATE.md**: 현 시점의 실제 구현 및 배포 상태, 기술부채, Blocker. (세션 완료 시 마다 반드시 갱신)
- **ROADMAP.md**: 장기 로드맵. (세션 완료 시 실제 진행 결과를 반영하여 갱신)
- **DEPLOYMENT.md**: 대상 레포지토리의 실제 배포 토폴로지 및 테스트 방법. (로그 검증, health check URL, Smoke test 명령어 등)

---

## 3. Bootstrap & Existing Repository Handling

1. **Bootstrap Procedure (신규 레포지토리 적용 시)**:
   - Target Repository 검증 시 `<target-repository>/.agents/` 디렉토리가 없으면 Coding Agent가 자율적으로 Bootstrap을 진행합니다.
   - Flow:
     ```text
     Target 확인
     ↓
     .agents/ 존재 여부 확인
     ↓
     없음
     ↓
     agi-with-gpt/agents/templates/ READ (Read-Only)
     ↓
     Target Repo 내부 <target-repository>/.agents/ 생성
     ↓
     Target Repo 코드베이스 및 git log 분석
     ↓
     PROJECT.md / STATE.md / ROADMAP.md / DEPLOYMENT.md 초기화 작성
     ↓
     Session 시작
     ```
2. **Existing Repository Handling**:
   - 이미 `<target-repository>/.agents/`가 존재하는 경우 기존 문서와 세션 이력을 절대 덮어쓰거나 훼손하지 않고 그대로 보존하고 이어서 사용합니다.

---

## 4. Session Plan과 Repository Roadmap 분리 원칙

- **ROADMAP.md**: 프로젝트 전체의 장기 계획
- **SESSION PLAN.md**: 이번 작업 세션 하나에서 수행할 구체적 실행 계획
- **분리 원칙**: 세션 하나를 진행한다고 해서 ROADMAP 전체를 갈아엎지 않으며, 세션 완료 후 실제 결과만 ROADMAP.md 및 STATE.md에 반영합니다.
