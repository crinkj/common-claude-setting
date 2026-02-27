# crinkj-plugins

> 프론트엔드 스킬과 AI 에이전트 팀 워크플로우를 위한 개인 Claude Code 플러그인 마켓플레이스

## 개요

모던 프론트엔드 개발과 멀티 에이전트 오케스트레이션을 위한 두 가지 Claude Code 플러그인입니다.

| 플러그인 | 커맨드 | 설명 |
|--------|---------|-------------|
| `frontend-skills` | `/frontend` | UI 빌드, 디자인 리뷰, Tailwind 레이아웃, Next.js 패턴을 하나의 커맨드로 |
| `dev-agents` | `/agent-team` | 3인 개발팀: team-lead → product-designer → fullstack-engineer |

---

## 설치

```bash
claude plugin install https://github.com/crinkj/common-claude-setting
```

---

## 플러그인

### `frontend-skills` — `/frontend`

프론트엔드 전체 워크플로우를 하나의 커맨드로 커버하는 통합 스킬입니다.

**4가지 모드:**

| 모드 | 설명 |
|------|------|
| **Build UI** | 범용 AI 스타일을 피한, 개성 있는 프로덕션급 UI 생성 |
| **Review UI** | [Web Interface Guidelines](https://github.com/vercel-labs/web-interface-guidelines) 기준으로 코드 감사, `파일:라인` 형식으로 결과 리포트 |
| **Tailwind Layouts** | CSS Grid, Flexbox, Container Queries, Scroll Snap, `clamp()` 등 고급 레이아웃 패턴 |
| **Next.js App Router** | Server/Client 컴포넌트, Server Actions, Parallel Routes, Suspense 스트리밍, 캐싱 전략 |

**사용 예시:**

```
/frontend 사이드바, 데이터 테이블, 다크모드가 있는 대시보드 만들어줘
/frontend review ./src/components/Card.tsx
/frontend tailwind 스티키 헤더가 있는 holy grail 레이아웃
/frontend next.js 스트리밍과 suspense를 활용한 서버 컴포넌트
```

---

### `dev-agents` — `/agent-team`

복잡한 개발 작업을 위해 3개의 에이전트를 순차적으로 오케스트레이션합니다. 각 에이전트는 역할에 최적화된 모델을 사용합니다.

```
team-lead → product-designer → fullstack-engineer → team-lead (최종 리뷰)
```

**에이전트 역할 및 모델:**

| 에이전트 | 모델 | 역할 |
|---------|------|------|
| `team-lead` | claude-opus-4-6 | 전략 기획, 태스크 분해, 리스크 분석, 최종 리뷰 |
| `product-designer` | claude-haiku-4-5 | UX 플로우, 정보 아키텍처, 컴포넌트 스펙 작성 |
| `fullstack-engineer` | claude-sonnet-4-6 | TypeScript/Next.js 구현, Prisma, 데이터 시각화 |

**동작 방식:**

- 팀 생성 전 사용자의 명시적 승인 필요 (Step -1)
- 에이전트 간 소통은 채팅이 아닌 핸드오프 파일(`.claude/handoff/`)로 처리
- `CLAUDE.md`로 모든 에이전트에게 프로젝트 컨텍스트 공유
- 각 에이전트는 자신의 담당 파일만 수정 가능
- 최종 리뷰 후 팀이 깔끔하게 종료 및 삭제됨

**사용 예시:**

```
/agent-team Prisma 데이터 레이어가 포함된 볼륨 프로파일 차트 구현
/agent-team 역할 기반 접근 제어가 있는 사용자 인증 추가
/agent-team API 레이어 리팩토링 및 통합 테스트 작성
```

---

## 디렉토리 구조

```
.
├── .claude-plugin/
│   └── marketplace.json          # 마켓플레이스 매니페스트
└── plugins/
    ├── frontend-skills/
    │   └── skills/frontend/
    │       └── SKILL.md          # /frontend 스킬 정의
    └── dev-agents/
        ├── agents/
        │   ├── team-lead.md
        │   ├── product-designer.md
        │   └── fullstack-engineer.md
        └── skills/agent-team/
            └── SKILL.md          # /agent-team 스킬 정의
```

---

## 설계 원칙

**토큰 효율** — 에이전트는 대화 히스토리 대신 최소한의 신선한 컨텍스트만 받습니다. 정보는 메시지가 아닌 파일로 전달됩니다.

**모델 매칭** — 고비용 모델(Opus)은 전략과 리뷰를, 빠른 모델(Haiku)은 디자인 스펙을, 균형잡힌 모델(Sonnet)은 구현을 담당합니다.

**스코프 강제** — 각 에이전트는 자신의 파일만 수정할 수 있습니다. team-lead는 코드를 작성하지 않고, engineer는 아키텍처 문서를 수정하지 않습니다.

**개성 있는 디자인** — `/frontend` 스킬은 남발된 폰트, 진부한 그라디언트, 판에 박힌 레이아웃을 명시적으로 지양합니다.

---

## 만든 사람

[crinkj](https://github.com/crinkj)
