# 01-security-and-isolation.md — 보안 및 격리 가드레일 (System Invariants)

> **CRITICAL / MANDATORY**: 본 레포지토리는 공개 또는 협업 환경에서 동작하며, 아래 가드레일은 어떠한 상황에서도 엄수해야 하는 **System Invariants**입니다.

---

## 1. 3대 절대 보안 금지 사항 (ZERO TOLERANCE)

1. **🚫 `.env` 파일 커밋 절대 금지**
   - 모든 `.env`, `.env.local`, `*.secret`, `credentials.json` 등 환경 설정 및 인증 파일은 `.gitignore`에 등록되어 있어야 하며 git tracking 대상에 포함되어선 안 됩니다.

2. **🚫 사설 IP 및 내부 토폴로지 노출 금지**
   - `192.168.x.x`, 내부 서버 IP, 맥주소, 사내 전용 도메인/포트 등을 이슈, 소스코드, 커밋 메시지, README 등에 절대 포함하지 않습니다.

3. **🚫 API Key 및 Private Credentials 유출 금지**
   - OpenAI API Key, Gemini API Key, GitHub Personal Access Token, SSH Private Key 등 어떠한 비밀 키도 코드베이스 및 세션 문서에 직접 하드코딩하지 않습니다.

---

## 2. 3대 경계 격리 가드레일 (Boundary Isolation Guardrails)

### 1) Repository Boundary Guardrail
- 지정된 **Target Repository 외 타 Repository 수정 금지**.
- Control Repository (`agi-with-gpt`)는 Target Repository 작업 실행 중 **READ-ONLY**입니다.
  - Target Repository 작업 중 `agi-with-gpt/agents/...` 하위에 어떠한 파일도 수정(WRITE)해서는 안 됩니다.
  - 모든 가변 상태(State, Roadmap, Active/Completed Sessions)는 오직 Target Repository 내 **`<target-repository>/.agents/`** 하위에만 생성 및 수정할 수 있습니다.
- 타 Repository의 파일 수정, `git add`, `git commit`, `git push`, branch 변경, dependency 수정은 절대 허용되지 않습니다.

### 2) Filesystem Boundary Guardrail
- 작업 가능한 파일 시스템 Writable 범위는 오직 **`repository_root/**`** 내부뿐입니다.
- Repository Root 바깥의 파일(`~/.zshrc`, `~/.bashrc`, `/etc`, `/usr/local`, Control Repo, 다른 프로젝트 디렉토리, 공용 NAS 등)을 수정해서는 안 됩니다.

### 3) Shared Infrastructure Guardrail
- 여러 프로젝트가 공유하는 Docker/DB/네트워크 등 파괴적인 인프라 명령은 명시적 허가 없이 금지됩니다:
  - 🚫 `docker system prune`
  - 🚫 `docker volume prune`
  - 🚫 `docker network prune`
  - 🚫 `docker container prune`
- 다른 프로젝트 소유의 container, volume, network, database, service를 삭제하거나 재시작해서는 안 됩니다.
- 오직 Target Repository가 소유한 전용 리소스만 조작해야 합니다.

### 4) Protected Infrastructure Repositories (Target Deny-List)
- **GoVail** (`govail`) 등 자율 코딩 시스템 자체가 의존하는 **LLM Infrastructure / API Provider**는 Target Repository 후보에서 명시적으로 제외되며, 어떠한 경우에도 수정 대상이 될 수 없습니다. (Bootstrap Dependency Protection)
  - 🚫 `GoVail` 소스코드 수정, `git commit`, `git push`, branch 변경 금지
  - 🚫 `GoVail` 컨테이너 재빌드, DB 변경, 환경 설정 변경, 서비스 중지/재시작 금지
  - 💡 `GoVail`은 오직 API 호출, Health Check, LLM 추론, 읽기 전용 상태 확인 목적으로만 사용 가능합니다.
  - 💡 `GoVail` 내부 문제가 발견되더라도 Target 레포지토리 작업 세션에서는 수정하지 않으며, `EXTERNAL_DEPENDENCY_ISSUE: GOVAIL`로 기록하고 Target 레포지토리 측에서 대응합니다.

---

## 3. 보안 검증 수칙 (Pre-commit Audit)

커밋 또는 PR을 올리기 전에 Worker Agent는 다음 명령 등으로 변경 사항을 전수 검사해야 합니다.

```bash
# 비밀 키 및 IP 패턴 검사 예시
git diff --cached | grep -iE "(api[_-]?key|secret|password|192\.168\.)"
```

위반 사항이 감지되면 즉시 커밋을 중단하고 시크릿을 제거하거나 환경 변수 참조 방식으로 전환해야 합니다.

