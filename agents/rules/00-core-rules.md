# 00-core-rules.md — 핵심 원칙 및 시스템 아키텍처

> **MANDATORY**: 모든 에이전트(Gemini, Local LLM 등)가 엄수해야 하는 핵심 작업 규칙입니다.

---

## 1. 언어 및 소통 규정

- **한국어 작성 원칙**: 사용자 대상 모든 설명, 문서(`.md`), 이슈 코멘트 및 코드 주석은 **한국어(Korean)**로 작성합니다.
- 기술 용어, 에러 로그 원본, 시스템 기호는 원문(English)을 유지합니다.

---

## 2. Prompt 해석 우선순위 (Prompt Interpretation Order)

Coding Agent가 프롬프트를 받았을 때는 다음 순서대로 명확히 판단하고 충돌을 해결합니다.

1. **Explicit Target Repository**: 명시된 Target Repository 및 Root 경로
2. **Explicit User Goal**: 프롬프트의 명시적 목표
3. **Repository Boundary**: 레포지토리 수정 경계 제약
4. **Repository SSOT**: 레포지토리 내 4대 문서 (`PROJECT.md`, `STATE.md`, `ROADMAP.md`, `DEPLOYMENT.md`)
5. **Current Code**: 실제 코드베이스 현황 (문서와 코드 충돌 시 코드 우선 확인 후 문서 update)
6. **Repository Roadmap**: 장기 계획
7. **Agent Inference**: 에이전트 자율 추론

---

## 3. 오케스트레이션 및 설계 철학 (Simplify & Extend)

- **No Over-Engineering**: Temporal, Message Queue, Kafka, 복잡한 Multi-Agent Orchestrator, 자동 Issue Polling/Scheduler, Vector DB 등을 추가하지 않습니다.
- **기존 구조 우선 (Extend > Rewrite)**:
  - `Extend > Rewrite`
  - `Simplify > Add abstraction`
  - `SSOT > Duplicate documentation`
  - `Deterministic workflow > Agent magic`
- Markdown, JSON, Git, Coding Agent, ChatGPT, User의 단순하고 명확한 조합으로 루프를 완성합니다.

---

## 4. 노드 역할 및 실행 환경

- **실행 환경**: `macmini` (Apple Silicon M4 서버).
- **vLLM 구동 금지**: `macmini` 환경에서는 고부하 vLLM 구동 등을 진행하지 않습니다.
- **Metal GPU 추론 / 호스트 네이티브**: 로컬 AI 추론(mlx-lm) 및 빌드는 호스트 네이티브로 실행합니다.
- **인프라 서비스**: DB, Redis 등 필요 시 Docker 컨테이너로 유연하게 구동합니다.

