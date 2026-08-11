# 📋 PLAN.md — {session-id}

> 이 작업의 실행 계획 및 검증 절차

## Context & Target

- **Session ID**: `{session-id}`
- **Target Repo**: `{target-repo}`
- **GitHub Issue**: `#{github-number}` ({github-issue-url})
- **Worker**: `{Gemini | Local LLM}`

## Summary

- {작업 배경 및 목표 요약 1~3줄}

## Steps

1. [ ] **조사 & 설계**: Target 레포지토리 연관 파일 분석
2. [ ] **계획 수립 & 리뷰**: `GOAL.md`, `PLAN.md` 작성 후 GitHub Issue Comment 등록 및 ChatGPT 리뷰 대기
3. [ ] **구현**: Target 코드 변경
4. [ ] **빌드 & 검증**: 단위 테스트, 실빌드, 런타임 동작 확인
5. [ ] **PR & 리뷰 반영**: PR 생성 및 Review 반영 후 승인 시 완료

## Build & Test Commands

```bash
# Target 레포지토리에서의 빌드 및 검증 명령
# 예: npm test, cargo test, pytest 등
```

## Verification Checklist

- [ ] 빌드 통과 여부
- [ ] 관련 테스트 통과 여부
- [ ] 보안 규칙 준수 (시크릿/IP 미노출)

## Risks & Boundaries

- {사이드이펙트 가능성 및 주의사항}
