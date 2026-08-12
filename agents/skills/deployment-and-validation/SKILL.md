# Deployment and Validation Skill

## Overview
Target Repository의 `DEPLOYMENT.md` 가이드에 맞춰 실제 프로젝트 배포를 수행하고, 배포 후 Smoke Test 및 런타임 검증을 실행하는 스킬입니다.

## Procedures
1. **DEPLOYMENT.md 확인**: Target Repo의 배포 토폴로지, 실행 스크립트, health check URL 조사.
2. **실배포 실행**: `npm run build`, `cargo build --release`, `docker compose up -d` 등 지정된 배포 명령어 수행.
3. **Smoke Test 실행**:
   - Health endpoint 또는 Smoke Test CLI 수행 (`curl -f http://localhost:<port>/health`)
   - 런타임 로그 및 컨테이너 가동 상태 확인
4. **실패 대응**: 배포 실패 시 reasonable 범위 내 원인 분석 및 수정 후 재배포. 구조적 장애 시 `BLOCKED` 처리.
