# 🚀 DEPLOYMENT.md — {target-repo}

> 본 문서는 레포지토리의 배포 환경, 서비스 명, 검증 URL, Smoke Test 방법 및 롤백 절차를 정의합니다. (민감한 Credential은 절대 기록하지 않습니다)

---

## 1. Deployment Topology

- **Target Environment**: `{Mac Mini | Docker Compose | GCP VM | Vercel | Cloud Run | Local Service}`
- **Service Name**: `{service-name}`
- **Runtime Host / Port**: `{host:port}`
- **Health Check Endpoint**: `http://localhost:{port}/health`

---

## 2. Deployment Procedure

```bash
# 배포 실행 스크립트 또는 명령어
# 예: docker compose up -d --build
```

---

## 3. Smoke Test & Post-Deployment Verification

```bash
# 배포 직후 런타임 검증을 위한 Smoke Test 스크립트 또는 curl 명령
curl -f http://localhost:{port}/health || exit 1
```

---

## 4. Rollback Plan

```bash
# 실패 시 롤백 수행 명령
```
