# Repository Analyst Skill

## Overview
Target Repository의 코드베이스 구조, 4대 장기 문서(`PROJECT`, `STATE`, `ROADMAP`, `DEPLOYMENT`), 빌드/테스트/배포 환경을 분석하고 **Second Inspection(자기 검수)**를 수행하는 스킬입니다.

## Checklist & Procedure
- [ ] **Target Repo & Boundary 검증**: Target Repo 경로(`repository_root`) 존재 및 Isolation 경계 준수 확인.
- [ ] **레포 4대 문서 조망**: `agents/repositories/<repo-name>/` 하위의 `PROJECT.md`, `STATE.md`, `ROADMAP.md`, `DEPLOYMENT.md` 읽기.
- [ ] **코드베이스 구조 조사**: 엔트리포인트, 주요 패키지, 빌드/테스트 스크립트 확인.
- [ ] **Second Inspection (v1 → Final Plan 자기검수)**:
  1. 이미 구현된 로직이 존재하는가?
  2. 수정 대상 파일 및 범위가 최소화되어 있는가?
  3. 현재 레포지토리 아키텍처와 충돌하지 않는가?
  4. 배포 방식 및 Smoke Test 방법이 `DEPLOYMENT.md`와 일치하는가?
  5. Repository Boundary / Filesystem Boundary를 지키고 있는가?
- [ ] **보안 점검**: `.env`, 사설 IP, 시크릿 하드코딩 여부 사전 확인.

