# 🏗️ PROJECT.md — GoVail/govail

> 본 문서는 GoVail 레포지토리의 변치 않는 장기 정의, 목적, 아키텍처 및 핵심 제약 사항을 정의하는 SSOT 문서입니다.

---

## 1. Project Overview

- **Repository**: `GoVail/govail`
- **Repository Root**: `/Users/yooncy/srv/govail`
- **Primary Purpose**: GoVail 고성능 라우팅, 에비던스 레이어 및 시스템 파이프라인 엔진.

---

## 2. Core Architecture

```text
Request Input → GoVail Router → Evidence Layer → Pipeline Core → Output Response
```

- **Core Tech Stack**: Rust / Go / Host Native Binaries
- **Key Modules**:
  - `router/`: 고성능 라우팅 및 릴레이
  - `evidence/`: 에비던스 수집 및 증명 레이어

---

## 3. Key Features

- [x] Evidence Layer 연동
- [ ] Router Regression 테스트 강화 및 런타임 안정성
- [ ] Mac Mini 배포 및 가동 상태 자동 검증

---

## 4. Architectural Constraints & Invariants

- 로컬 Mac Mini Host Native 구동
- Target Root `/Users/yooncy/srv/govail` 외 바깥 파일 수정 절대 금지
