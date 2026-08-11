# GitHub Workflow Skill

## Overview
GitHub CLI (`gh`) 또는 Git 명령을 사용하여 Issue Comment 등록, PR 생성, 상태 갱신을 진행하는 스킬입니다.

## Instructions
1. **Issue Comment 작성**:
   - `agents/templates/ISSUE_COMMENT.template.md` 양식 참조.
   - Plan 작성 후 해당 Target Repo의 Issue에 Comment 남기기:
     ```bash
     gh issue comment <issue-number> --repo yooncy/<target-repo> --body-file comment.md
     ```

2. **PR 생성**:
   - `agents/templates/PR_DESCRIPTION.template.md` 양식 참조.
   - 실빌드 및 실검증 로그를 명시하여 PR 제출:
     ```bash
     gh pr create --repo yooncy/<target-repo> --title "<Title>" --body-file pr_body.md
     ```
