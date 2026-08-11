# 00-core-rules.md — 핵심 원칙 및 시스템 아키텍처

> **MANDATORY**: 모든 에이전트(Gemini, Local LLM 등)가 엄수해야 하는 핵심 작업 규칙입니다.

---

## 1. 언어 및 소통 규정

- **한국어 작성 원칙**: 사용자 대상 모든 설명, 문서(`.md`), 이슈 코멘트 및 코드 주석은 **한국어(Korean)**로 작성합니다.
- 기술 용어, 에러 로그 원본, 시스템 기호는 원문(English)을 유지합니다.

---

## 2. 노드 역할 및 실행 환경

- **실행 환경**: `macmini` (Apple Silicon M4 서버).
- **vLLM 구동 금지**: `macmini` 환경에서는 고부하 vLLM 구동 등을 진행하지 않습니다.
- **Metal GPU 추론 / 호스트 네이티브**: 로컬 AI 추론(mlx-lm) 및 빌드는 호스트 네이티브로 실행합니다.
- **인프라 서비스**: DB, Redis 등 필요 시 Docker 컨테이너로 유연하게 구동합니다.

---

## 3. 검증 게이트 (Build-Deploy-Verify Gate)

작업 완료 선언 및 PR 생성 전에는 반드시 다음 단계를 검증해야 합니다.

1. **실제 빌드/체크**: Target Repository에 맞는 빌드 또는 린트 명령 수행 (`npm run build`, `cargo check`, `pytest` 등).
2. **실제 검증**: 수정된 로직이 요구사항에 부합하는지 런타임 테스트 또는 단위 테스트 실행.
3. **증거 첨부**: 검증 결과 로그 및 상태를 세션 및 PR 설명에 명시.

---

## 4. 오케스트레이션 설계 철학

- **No Over-Orchestration**: 불필요한 daemon, webhook, polling 오케스트레이션 코드를 새로 만들지 않습니다.
- 기존 GitHub Issue, PR, Markdown 문서, 깃 명령, LLM 도구를 조합한 **협업 루프(Collaboration Loop)**를 최우선 적용합니다.
