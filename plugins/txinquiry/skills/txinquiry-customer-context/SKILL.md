---
name: txinquiry-customer-context
description: TX Inquiry 고객·주문 배경 조회 — 고객 검색과 정보, 과거 문의 이력, 같은 주문의 이전 문의, 주문·화물 정보 조회, 셀러 요약, 창고 목록, 문의에 주문 연결. "이 고객 전에도 문의했었나", "주문 상태 확인해줘", "이 문의에 주문 연결해줘", customer history, order info, link order 요청에 사용.
---

# TX Inquiry 고객·주문 배경

문의에 답하기 전에 **누가·무엇에 대해 묻는지**를 확인한다. 같은 질문이 반복되는지, 주문이 어떤
상태인지 모르고 답하면 오답이 나간다.

## 1. 고객 확인

- `search_customers` — 이름·연락처 등으로 고객을 찾는다.
- `get_customer_info` — 고객 기본 정보.
- `get_seller_summary` — 셀러 고객이면 계정 요약.

## 2. 이력 확인 (오답 방지의 핵심)

- `get_customer_past_inquiries` — 이 고객의 과거 문의. **같은 건으로 다시 물은 것인지** 먼저 본다.
- `get_same_order_past_inquiries` — 같은 주문으로 들어온 다른 문의. 중복 응대·엇갈린 답변을 막는다.

## 3. 주문·화물 확인

- `search_orders` — 주문 검색.
- `get_inquiry_order_info` — 이 문의에 연결된 주문 정보.
- `get_cargo_order_info` — 화물(카고) 건 정보.
- `list_warehouses` — 창고 후보값.

## 4. 연결 (쓰기 — `confirm=true` 필요)

- `set_inquiry_order_info` — 문의에 주문을 연결·수정한다. **어떤 주문을 붙이는지 주문번호를 보여주고**
  동의를 받은 뒤 실행한다. 잘못 연결하면 이후 이력 조회가 전부 어긋난다.
- `set_freight_order_ticket` — 화물 주문과 티켓을 연결한다.

연결 후 `get_inquiry_order_info` 로 재조회해 실제로 붙었는지 확인한다.

## 규칙

- 고객 개인정보는 **답변에 필요한 최소한만** 사용한다. 조회한 연락처·주소를 요약에 그대로 나열하지 않는다.
- 과거 이력을 근거로 답할 때는 "언제 어떤 답변이 나갔다"는 사실만 쓰고 추측을 덧붙이지 않는다.
- 주문 상태는 조회 시점 기준이다. 답변에 조회한 시점을 함께 밝힌다.
- 이 서버의 조회 도구는 기간 기본값이 없다 — 이력 조회 범위를 정하면 그 범위를 답변에 밝힌다.
