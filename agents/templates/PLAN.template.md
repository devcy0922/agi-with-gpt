# 📋 PLAN.md — {session-id}

> 이 작업의 실행 계획, 자기 검수(Second Inspection), 검증 및 배포 절차

## Context & Target

- **Session ID**: `{session-id}`
- **Target Repo**: `{target-repo}`
- **Repository Root**: `{repository-root}`
- **Worker**: `{Gemini | Local LLM}`

## Summary

- {작업 배경 및 목표 요약 1~3줄}

## Implementation Strategy & Steps

1. [ ] **Repository Analysis**: 레포 4대 문서 및 소스코드 분석
2. [ ] **PLAN v1 수립**: 실행 계획 작성
3. [ ] **Second Inspection (자기 검수)**: 실제 소스코드 재검토 및 PLAN Final 확정
4. [ ] **코드 구현**: Target Repository 내부 소스 변경
5. [ ] **빌드 & 자동 테스트**: 단위/통합 테스트 통과 확인
6. [ ] **실배포**: `DEPLOYMENT.md` 가이드에 맞춘 실배포 수행
7. [ ] **배포 후 검증 (Smoke Test)**: Endpoint / Log / User Flow 런타임 검증
8. [ ] **문서 갱신**: `STATE.md`, `ROADMAP.md` 업데이트
9. [ ] **Session Archive**: 완료 세션 아카이브 이동 및 요약/추천 작성

## Second Inspection Checklist (v1 → Final Plan)

- [ ] 이미 구현된 기능 여부 확인
- [ ] 수정 대상 파일 및 범위의 적절성 확인
- [ ] Repository Boundary 및 Filesystem Boundary 위반 여부 확인
- [ ] 배포 및 Smoke Test 방법 확정

## Build & Test & Deployment Commands

```bash
# Target 레포지토리에서의 빌드, 테스트, 배포 명령
```

## Verification & Smoke Test Strategy

- [ ] 빌드 및 단위 테스트 통과
- [ ] 실배포 후 Smoke Test / Health Endpoint 통과
- [ ] 보안 가드레일 준수 (시크릿/IP 미노출)

## Risks & Out of Scope

- {사이드이펙트 가능성 및 주의사항}

