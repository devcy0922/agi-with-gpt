# GitHub Workflow Skill

## Overview
Git 명령 및 GitHub CLI를 사용하여 작업 브랜치 관리, 커밋, 푸시 및 사용자 승인 기반 Git Tag 생성을 처리하는 스킬입니다.

## Instructions
1. **Commit & Push**:
   - 보안 체크 수행 (`git diff --cached | grep -iE "(api[_-]?key|secret|password|192\.168\.)"`)
   - 작업 완료 후 변경 내용을 명확한 커밋 메시지와 함께 커밋 후 푸시:
     ```bash
     git add .
     git commit -m "feat(<target-repo>): <clear commit message>"
     git push origin main
     ```

2. **Git Tag Policy (MANDATORY)**:
   - **자율 작업 Loop 중 임의 Git Tag 생성 절대 금지**.
   - 사용자가 명시적으로 *"이 버전을 확정한다."*, *"태그 찍자."*, *"v0.x로 확정."* 등의 메시지를 남겼을 때만 Tag 생성:
     ```bash
     git tag -a v0.x.y -m "Release v0.x.y confirmed by user"
     git push origin v0.x.y
     ```

