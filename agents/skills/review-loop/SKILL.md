# Review Loop Skill

## Overview
ChatGPT Reviewer의 피드백을 수용하고 세션 상태와 코드를 반복적으로 개선하는 스킬입니다.

## Review States
- `APPROVED_FOR_NEXT_STAGE`: 다음 단계(구현 또는 PR)로 진행.
- `CHANGES_REQUESTED`: 지적된 사항을 `GOAL.md`, `PLAN.md` 또는 코드에 반영 후 다시 리뷰 요청.

## Action Steps
1. ChatGPT feedback 코멘트 전문 읽기.
2. 지적받은 항목별 수정 내용 세션 문서 및 `state.json` decisionLog에 반영.
3. 수정 완료 후 issue comment 또는 PR thread에 수정 완료 알림 등록.
