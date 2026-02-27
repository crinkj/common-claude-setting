---
name: agent-team
description: team-lead → product-designer → fullstack-engineer 순서로 에이전트 팀을 구성해 작업을 실행합니다. 사용법: /agent-team [작업 내용]
---

# Agent-Team Skill

사용자가 요청한 작업을 3인 에이전트 팀으로 실행한다.

---

## 모델 배정 (역할별 최적화)

| 에이전트 | 모델 | 이유 |
|----------|------|------|
| team-lead | `claude-opus-4-6` | 전략 기획·리스크 분석·복잡한 판단 |
| product-designer | `claude-haiku-4-5-20251001` | 구조화된 설계 스펙 작성 (비용 최적) |
| fullstack-engineer | `claude-sonnet-4-6` | 복잡한 TypeScript/Next.js 구현 |

---

## 토큰 최적화 전략

### 원칙
1. **신규 태스크 = 신선한 컨텍스트**: 각 에이전트는 독립 스폰 → 이전 대화 기록 미전달
2. **반복 컨텍스트 → CLAUDE.md**: 프로젝트 스펙, 소스 규칙, 기술 스택은 CLAUDE.md에 상주하여 모든 에이전트가 자동 로드 (spawn prompt 재작성 금지)
3. **인계 파일로 통신**: 대화 메시지 대신 `.claude/handoff/*.md` 파일로 정보 전달
4. **스폰 프롬프트 최소화**: 이번 단계에서 새롭게 추가된 정보만 포함. CLAUDE.md에 있는 내용 재작성 금지
5. **Haiku for design**: 설계 스펙 작성은 Haiku로 비용 절감, 복잡한 코딩·분석은 Sonnet 사용
6. **단계 완료 후 신규 스폰**: 에이전트 재사용보다 신규 스폰이 컨텍스트 오염 방지

---

## 실행 순서

### [필수] Step -1: 플랜 제시 후 사용자 승인 대기

에이전트 스폰 전에 반드시 아래 형식으로 플랜을 사용자에게 먼저 보여준다.
**사용자가 명시적으로 동의("응", "ㅇㅋ", "진행해", "go" 등)할 때까지 Step 0 이후 절대 실행하지 않는다.**

```
## 실행 플랜

**목표**: {작업 한 줄 요약}

**에이전트 순서**:
1. team-lead (Opus) — 태스크 분해 & 설계 지시
2. product-designer (Haiku) — UX/컴포넌트 스펙
3. fullstack-engineer (Sonnet) — 실제 구현
4. team-lead (Opus) — 최종 리뷰

**주요 작업 범위**:
- {In-scope 항목 3~5개}

**예상 산출물**:
- {파일/기능 목록}

**제외 범위**:
- {Out-of-scope 항목}

승인하시면 팀 구성을 시작합니다.
```

---

### 사전 준비

`.claude/handoff/` 디렉토리가 없으면 생성한다.

---

### Step 0: 팀 생성

TeamCreate로 팀을 만들고 리더(orchestrator) 역할을 맡는다.

```
team_name: "microcap-radar"
description: "Microcap Radar 에이전트 팀"
```

---

### Step 1: team-lead 스폰

모델: `claude-sonnet-4-6`
정의 파일: `~/.claude/agents/team-lead.md`

**스폰 프롬프트** (최소화 — CLAUDE.md 내용 재작성 금지):
```
TASK: {사용자 요청 한 줄 요약}

ACTION:
1. CLAUDE.md를 읽고 프로젝트 컨텍스트 파악
2. 태스크를 product-designer와 fullstack-engineer가 실행할 수 있도록 분해
3. 결과를 .claude/handoff/team-lead-output.md에 저장
4. 완료 후 리더에게 메시지 전송
```

team-lead 완료 메시지 수신 후 Step 2 진행.

---

### Step 2: product-designer 스폰

**조건**: `.claude/handoff/team-lead-output.md` 존재 확인 후 스폰.
모델: `claude-haiku-4-5-20251001`
정의 파일: `~/.claude/agents/product-designer.md`

**스폰 프롬프트** (최소화):
```
ACTION:
1. CLAUDE.md 읽기 (프로젝트 컨텍스트 — 기술 스택, 초보자 UX 원칙)
2. .claude/handoff/team-lead-output.md 읽기 (태스크 지시)
3. UX 플로우, 컴포넌트 스펙, 반응형 레이아웃 설계
4. .claude/handoff/designer-output.md에 저장
5. 완료 후 리더에게 메시지 전송
```

product-designer 완료 메시지 수신 후 Step 3 진행.

---

### Step 3: fullstack-engineer 스폰

**조건**: `.claude/handoff/designer-output.md` 존재 확인 후 스폰.
모델: `claude-sonnet-4-6`
정의 파일: `~/.claude/agents/fullstack-engineer.md`

**스폰 프롬프트** (최소화):
```
ACTION:
1. CLAUDE.md 읽기 (기술 스택, 소스 규칙, 점수 모델, Mock 데이터 형식)
2. .claude/handoff/team-lead-output.md 읽기 (태스크 목록 + 완료 기준)
3. .claude/handoff/designer-output.md 읽기 (컴포넌트 스펙)
4. MVP 1차 구현:
   - Prisma schema (scoreBreakdownJson 포함)
   - Mock 데이터 (sourceName/sourceUrl 필수)
   - 메인 대시보드 + 상세 페이지 UI
   - 차트 (Client Component)
5. 완료 후 리더에게 메시지 전송
```

---

### Step 4: team-lead 검토 (신규 스폰 또는 메시지)

fullstack-engineer 완료 후 team-lead에게 최종 리뷰 요청.

**메시지 형식**:
```
구현 완료. 아래 항목 검토 후 .claude/handoff/review-output.md에 저장:
1. CLAUDE.md 신뢰 소스 규칙 준수 여부 (출처 없는 Mock 데이터 있는가)
2. Score 계산 가중치 정합성 (Volume Spike / SEC / Afterhours 표 기준)
3. Prisma schema scoreBreakdownJson 필드 존재
4. Mock 데이터 sourceName/sourceUrl 포함
5. 차트 컴포넌트에 'use client' 선언 여부
검토 완료 후 리더에게 메시지 전송.
```

---

### Step 5: 정리

```
1. shutdown_request 순서: fullstack-engineer → product-designer → team-lead
2. 모든 완료 확인 후 TeamDelete 실행
```

---

## 단계 전환 체크리스트

각 단계 전환 전 확인:
- [ ] 이전 에이전트 완료 메시지 수신
- [ ] 핸드오프 파일 존재 (`ls .claude/handoff/`)
- [ ] 다음 스폰 프롬프트에 CLAUDE.md 내용 재작성 없음

---

## 오류 처리

| 상황 | 대응 |
|------|------|
| 에이전트 응답 없음 | Shift+Up/Down으로 직접 확인 후 추가 지시 |
| 핸드오프 파일 없음 | 해당 에이전트에게 재실행 요청 |
| 파일 충돌 | CLAUDE.md 파일 소유권 규칙 재전달 |
| 품질 미달 | team-lead 피드백 후 해당 에이전트 재지시 |
| 토큰 과다 | 해당 에이전트 종료 후 신규 스폰, 핸드오프 파일로 재시작 |

---

## 활용 가능한 스킬

| 에이전트 | 스킬 |
|----------|------|
| product-designer | `web-design-guidelines`, `tailwindcss-advanced-layouts` |
| fullstack-engineer | `nextjs-app-router-patterns`, `tailwindcss-advanced-layouts`, `web-design-guidelines` |

---

## 주의사항

- 에이전트 간 정보 전달은 **파일(handoff)** 로만. 대화 메시지로 전체 컨텍스트 전달 금지
- CLAUDE.md 변경 시 모든 에이전트가 자동으로 새 컨텍스트 로드
- 팀 정리는 반드시 **리더**가 실행. 팀원의 TeamDelete 실행 금지
- 각 에이전트는 **소유 파일만** 수정 (CLAUDE.md 파일 소유권 테이블 참고)
