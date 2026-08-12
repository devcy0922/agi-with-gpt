# 🚀 DEPLOYMENT.md — devcy0922/agi-with-gpt

> 본 문서는 `agi-with-gpt` 레포지토리의 배포 환경, 검증 방법 및 롤백 절차를 정의합니다.

---

## 1. Deployment Topology

- **Target Environment**: Mac Mini Host Native Git Workspace
- **Repository Root**: `/Users/yooncy/srv/agi-with-gpt`
- **Git Remote**: `origin` (`devcy0922/agi-with-gpt`)
- **Primary Branch**: `main`

---

## 2. Deployment Procedure

`agi-with-gpt`는 오케스트레이션 프레임워크 문서 및 스크립트 모음이므로 배포 과정은 Git Commit & Git Push로 이루어집니다.

```bash
# 1. 시크릿 및 사설 IP 사전 검사
git diff --cached | grep -iE "(api[_-]?key|secret|password|192\.168\.)"

# 2. 커밋 및 원격 저장소 푸시
git commit -m "feat: updated autonomous coding loop framework"
git push origin main
```

---

## 3. Smoke Test & Post-Deployment Verification

```bash
# 규칙 및 문서 무결성 검증
test -f AGENTS.md && test -f agents/rules/00-core-rules.md && test -f agents/rules/01-security-and-isolation.md && test -f agents/rules/02-session-lifecycle.md && test -f agents/rules/03-repository-management.md && test -f agents/rules/04-execution-and-deployment.md && echo "SSOT Framework Verified"
```

---

## 4. Rollback Plan

```bash
git reset --hard HEAD~1
git push origin main --force-with-lease
```
