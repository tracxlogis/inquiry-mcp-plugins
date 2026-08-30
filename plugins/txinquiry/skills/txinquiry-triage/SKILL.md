---
name: txinquiry-triage
description: TX Inquiry 문의 조회·분류·배정 — 문의 목록/검색(ES 포함), 유형·상태·태그·담당자 후보 조회, 상담원 배정과 대기열 배정·종료, 유형/태그/관심·즐겨찾기 설정. "미배정 문의 보여줘", "이 문의 나한테 배정해줘", "대기열 처리", assign inquiry, search inquiries, wait queue 요청에 사용.
---

# TX Inquiry 문의 조회·분류·배정

들어온 문의를 **찾고 → 분류하고 → 담당자에게 붙이는** 흐름을 수행한다.
순서: **① 목록·검색으로 대상 확정 → ② 분류(유형·태그) → ③ 배정 → ④ 재조회로 반영 확인**.

## 1. 찾기

- `list_inquiries` — 기본 목록 조회. 상태·기간·담당자 조건으로 좁힌다.
- `search_inquiries_es` — 키워드 전문 검색. 본문까지 찾아야 할 때.
- `list_inquiries_by_ids` — 티켓 번호를 이미 알고 있을 때 한 번에 조회.
- 후보값: `list_inquiry_types`, `list_inquiry_statuses`, `list_inquiry_tags`, `search_inquiry_tags`,
  `list_inquiry_operators`
- 대기열: `list_wait_queue` — 배정 대기 중인 고객 목록.

## 2. 분류·배정 (쓰기 — 반드시 아래 절차)

이 서버의 **쓰기 도구는 전부 `confirm=true` 를 요구**한다. 실행 전 **대상 티켓과 바꿀 값을 요약해
보여주고 사용자 동의를 받은 뒤** 호출한다.

| 하려는 일 | 도구 |
|---|---|
| 담당 상담원 지정·변경 | `assign_inquiry_operator` |
| 대기열 고객을 상담원에게 배정 | `assign_wait_queue` |
| 대기열 건 종료 | `close_wait_queue` |
| 문의 유형(대분류) 지정 | `set_inquiry_main_type` |
| 태그 설정 | `set_inquiry_tags` |
| 관심 상담원(watcher) 지정 | `set_inquiry_watcher` |
| 즐겨찾기 표시 | `set_inquiry_favorite` |

- 여러 건을 한 번에 바꿀 때는 건수와 샘플 3~5건을 먼저 보여주고 범위를 확정받는다.
- 배정은 다른 상담원의 업무량에 영향을 준다 — 누구에게 붙이는지 이름을 밝히고 동의를 받는다.

## 3. 확인

처리 후 **같은 조건으로 재조회**해 실제로 반영됐는지 확인하고 결과를 보고한다.
일부만 성공했으면 성공·실패 건수를 나눠 사실대로 알린다.

## 조회 기준

- 기간·상태 기본값을 서버가 정해 주지 않는다. 조건을 지정하지 않으면 먼저 제시하고,
  **답변에 사용한 기간·상태·담당자 범위를 밝힌다.**
- 화면 건수와 다르면 기간, 상태 필터, "내 담당만/팀 전체" 범위부터 대조한다.
- 권한은 호출 시점에 서버가 판정한다. 권한이 없어 거절되면 우회하지 말고 그대로 알리고
  필요한 권한을 안내한다.
