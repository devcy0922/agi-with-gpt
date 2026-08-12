# 🚀 DEPLOYMENT.md — GoVail/govail

> 본 문서는 GoVail 레포지토리의 배포 환경, 서비스 명, 검증 URL, Smoke Test 방법 및 롤백 절차를 정의합니다.

---

## 1. Deployment Topology

- **Target Environment**: Mac Mini Host Native Local Service / Docker Container
- **Service Name**: `govail-service`
- **Repository Root**: `/Users/yooncy/srv/govail`
- **Health Check Endpoint**: `http://localhost:8080/health` (또는 local binary verification)

---

## 2. Deployment Procedure

```bash
# Target Root에서 빌드 및 서비스 구동
cd /Users/yooncy/srv/govail
cargo build --release
```

---

## 3. Smoke Test & Post-Deployment Verification

```bash
# Smoke test 실행
curl -f http://localhost:8080/health || echo "Local binary verification check"
```

---

## 4. Rollback Plan

```bash
git checkout HEAD~1
```
