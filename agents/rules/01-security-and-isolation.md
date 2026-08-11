# 01-security-and-isolation.md — 보안 및 정보 격리 규정

> **CRITICAL / MANDATORY**: 본 레포지토리는 GitHub `yooncy` 계정의 **Public Repository**로 공개됩니다.
> 보안 위반 요소는 어떠한 경우에도 커밋되거나 유출되어서는 안 됩니다.

---

## 1. 3대 절대 금지 사항 (ZERO TOLERANCE)

1. **🚫 `.env` 파일 커밋 절대 금지**
   - 모든 `.env`, `.env.local`, `*.secret`, `credentials.json` 등 환경 설정 및 인증 파일은 `.gitignore`에 등록되어 있어야 하며 git tracking 대상에 포함되어선 안 됩니다.

2. **🚫 사설 IP 및 내부 토폴로지 노출 금지**
   - `192.168.x.x`, 내부 서버 IP, 맥주소, 사내 전용 도메인/포트 등을 이슈, 소스코드, 커밋 메시지, README 등에 절대 포함하지 않습니다.

3. **🚫 API Key 및 Private Credentials 유출 금지**
   - OpenAI API Key, Gemini API Key, GitHub Personal Access Token, SSH Private Key 등 어떠한 비밀 키도 코드베이스 및 세션 문서에 직접 하드코딩하지 않습니다.

---

## 2. 보안 검증 수칙 (Pre-commit Audit)

커밋 또는 PR을 올리기 전에 Worker Agent는 다음 명령 등으로 변경 사항을 전수 검사해야 합니다.

```bash
# 비밀 키 및 IP 패턴 검사 예시
git diff --cached | grep -iE "(api[_-]?key|secret|password|192\.168\.)"
```

위반 사항이 감지되면 즉시 커밋을 중단하고 시크릿을 제거하거나 환경 변수 참조 방식으로 전환해야 합니다.
