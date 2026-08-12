# 🚀 DEPLOYMENT.md — devcy0922/agi-with-gpt

> 본 문서는 `agi-with-gpt` 레포지토리의 배포 환경, 토폴로지 및 검증 절차를 정의하는 SSOT 문서입니다.

---

## 1. Deployment Topology

- **Environment**: macOS Host (Mac mini M4)
- **Deployment Mode**: Local Orchestrator / Control Plane (Host Native)
- **Target Repository Path**: `/Users/yooncy/srv/agi-with-gpt`

---

## 2. Deployment Instructions

본 레포지토리는 에이전트 협업 및 오케스트레이션 규칙 저장소(Control Plane)입니다.

- **Sync Strategy**: Git Commit & Remote Main Push
  ```bash
  git status
  git add .
  git commit -m "feat(arch): ..."
  git push origin main
  ```

---

## 3. Verification & Smoke Test

1. **Structure Integrity Check**:
   - `AGENTS.md` 및 `agents/rules/` 유효성 점검
   - `.agents/` 디렉토리 내 4대 문서 및 세션 유효성 점검
2. **Git Status Clean Check**:
   - `git status`로 트래킹되지 않거나 누락된 파일 없는지 확인
