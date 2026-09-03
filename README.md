# TX Inquiry MCP Plugin

**한국어** | [English](#english)

TX Inquiry(CS 문의) 시스템을 AI 어시스턴트에 연결하는 **사내용** 플러그인입니다. 설치하면 MCP 연결과
함께 상담 업무 스킬(대기열·티켓 처리·고객 배경·상담원 운영·업무 브리핑)이 한 번에 설정됩니다.

설치 후에는 이렇게 물어보면 됩니다. *"지금 상담 현황 어때?"*, *"이 티켓 상세 보여줘"*,
*"이 고객 전에도 문의했었나?"*

## 사용 전제

- **TX 관리자 계정** (평소 admin 에 로그인하는 그 계정)
- **사내망 접근** — 운영 MCP 는 사내 도메인이라 회사망 또는 VPN 이 필요합니다.
- **GitHub 접근** — 마켓플레이스는 GitHub 공개 저장소 `tracxlogis/inquiry-mcp-plugins`(표준 `owner/repo` 표기)로
  배포합니다. GitHub 계정이나 별도 인증 없이 인터넷만 있으면 등록·설치할 수 있습니다.
- 클라이언트 중 하나: Claude Code, Codex CLI, Claude 데스크톱 앱, ChatGPT 데스크톱 앱

## 설치

### Claude Code

```
/plugin marketplace add tracxlogis/inquiry-mcp-plugins
/plugin install txinquiry@tx-inquiry-mcp-marketplace
```

### Codex CLI

```
codex plugin marketplace add tracxlogis/inquiry-mcp-plugins
codex plugin add txinquiry@tx-inquiry-mcp-marketplace
```

### 데스크톱 앱

Claude 데스크톱 앱은 **설정 → 플러그인 → 추가 → 저장소에서 추가**, ChatGPT 데스크톱 앱은
**플러그인 → 추가 → 플러그인 마켓플레이스 추가** 에서 위 저장소 주소를 넣습니다.

## 인증

첫 사용 때 둘 중 하나로 인증합니다.

- **OAuth 로그인** — 브라우저가 열리면 TX 관리자 계정으로 로그인하고 접근 권한에 동의합니다.
- **개인 API 키** — 브라우저 왕복 없이 쓰려면 tx-admin-react 의 `/my-profile` 에서 발급한
  개인 키(`txak_` 로 시작)를 클라이언트 설정에 넣습니다. 키는 본인 것이며 공유하지 않습니다.

## 포함된 스킬

| 스킬 | 다루는 일 |
|---|---|
| `txinquiry-briefing` | 내 담당 건수, 대기열, 오늘 로스터를 모아 우선순위로 보고 |
| `txinquiry-triage` | 문의 검색·분류, 담당자 배정, 대기열 배정·종료 |
| `txinquiry-handling` | 티켓 상세·AI 요약, 답변 등록, 상태 전이, 메일 재발송·템플릿 |
| `txinquiry-customer-context` | 고객 정보, 과거 문의 이력, 주문·화물 조회와 문의-주문 연결 |
| `txinquiry-agent-ops` | 상담원 설정·로스터·우선배정, 상태 변경, 집계, 스팸 정책 |

## 안전장치

- **데이터를 바꾸는 작업은 모두 명시적 확인이 필요합니다.** 어시스턴트가 대상과 바꿀 값을 요약해
  보여주고, 동의한 뒤에만 실행됩니다.
- 고객에게 발송되는 답변·메일은 문구를 먼저 보여주고 동의를 받습니다.
- 권한은 호출 시점에 서버가 판정합니다. 화면에서 권한이 없는 기능은 여기서도 거절됩니다.
- 조회 기간 기본값은 서버가 정해 주지 않습니다. 어시스턴트가 사용한 기간·조건을 답변에 밝힙니다.

## 문의

연결 가이드와 도구 목록: <https://inquiry.tracxlogis.com/docs/mcp>

---

## English

[한국어](#tx-inquiry-mcp-plugin) | **English**

An **internal** plugin that connects the TX Inquiry (customer support) system to your AI assistant.
Installing it sets up the MCP connection together with support task skills — wait queue, ticket
handling, customer background, agent operations, and a daily briefing.

Once installed you can ask: *"How do our inquiries look right now?"*, *"Show me this ticket"*,
*"Has this customer contacted us before?"*

### Requirements

- A **TX admin account** — the one you normally use to sign in to admin.
- **Internal network access** — the production MCP server is on an internal domain, so you need the
  office network or VPN.
- **GitHub access** — the marketplace is distributed from the public GitHub repository
  `tracxlogis/inquiry-mcp-plugins` (standard `owner/repo` form). Registering and installing need only an
  internet connection; no GitHub account or extra authentication is required.
- One of these clients: Claude Code, Codex CLI, Claude desktop app, ChatGPT desktop app.

### Install

Claude Code:

```
/plugin marketplace add tracxlogis/inquiry-mcp-plugins
/plugin install txinquiry@tx-inquiry-mcp-marketplace
```

Codex CLI:

```
codex plugin marketplace add tracxlogis/inquiry-mcp-plugins
codex plugin add txinquiry@tx-inquiry-mcp-marketplace
```

For the desktop apps, add the same repository URL from **Settings → Plugins → Add → Add from
repository** (Claude) or **Plugins → Add → Add plugin marketplace** (ChatGPT).

### Authentication

- **OAuth sign-in** — a browser window opens; sign in with your TX admin account and approve access.
- **Personal API key** — to skip the browser round trip, issue a personal key (it starts with `txak_`)
  from `/my-profile` in tx-admin-react and put it in your client configuration. The key is yours
  alone; do not share it.

### Included skills

| Skill | Covers |
|---|---|
| `txinquiry-briefing` | Your open tickets, the wait queue, and today's roster in priority order |
| `txinquiry-triage` | Search and classify inquiries, assign agents, handle the wait queue |
| `txinquiry-handling` | Ticket detail and AI summary, replies, status changes, email templates |
| `txinquiry-customer-context` | Customer info, past inquiries, order and cargo lookup, order linking |
| `txinquiry-agent-ops` | Agent settings, roster, priority assignment, daily rebuilds, spam policy |

### Safeguards

- **Every action that changes data requires explicit confirmation.** The assistant summarizes the
  targets and the new values, and runs only after you agree.
- Replies and emails that reach customers are shown to you before they are sent.
- Permissions are evaluated by the server on each call. Anything you cannot do on the screens is
  refused here too.
- The server does not apply default query periods. The assistant states the period and filters it used.

### Support

Connection guide and tool list: <https://inquiry.tracxlogis.com/docs/mcp>
