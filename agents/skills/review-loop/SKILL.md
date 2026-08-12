# Review Loop Skill

## Overview
ChatGPT의 작업 프롬프트를 수신하여 자율 작업 사이클을 완료한 뒤, 결과를 요약하고 다음 Action 3개를 추천하여 사용자가 검증할 수 있도록 전달하는 대화형 루프 스킬입니다.

## Workflow Rules
1. **Approval-Free Auto Execution**:
   - 프롬프트 수신 후 PLAN 수립 → Second Inspection → 구현 → 빌드 → 테스트 → 배포 → Smoke Test까지 중간 승인 없이 자율 완수.
2. **Current Status & Recommended Next Actions**:
   - 작업 완수 후 `STATE.md`, `ROADMAP.md`를 업데이트하고 세션을 `completed`로 이동.
   - `## Current Status` 및 `## Recommended Next Actions` (정확히 3개)를 작성.
3. **User Validation Boundary**:
   - 다음 Action 3개 제안 출력을 마지막으로 자율 실행을 멈추고 사용자가 검증 결과를 ChatGPT에 전달하도록 전이.

