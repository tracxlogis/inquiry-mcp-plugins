---
name: txinquiry-handling
description: TX Inquiry 티켓 처리 — 문의 상세·AI 요약 확인, 답변 등록, 상태 전이, 고객 메일 재발송, 메일 템플릿 조회·저장, 신규 문의 생성, 지식 검색. "이 문의 답변해줘", "티켓 상세 보여줘", "상태 완료로 바꿔줘", "메일 다시 보내줘", add reply, change status, email template 요청에 사용.
---

# TX Inquiry 티켓 처리

한 건의 문의를 **읽고 → 답변하고 → 상태를 정리하는** 흐름을 수행한다.

## 1. 읽기

- `get_inquiry_detail` — 티켓 본문·대화 이력·속성 확인. 처리의 시작점이다.
- `get_ai_inquiry_summary` — 대화가 길 때 요약으로 맥락 파악.
- `search_inquiry_knowledge` / `get_inquiry_knowledge` — 답변 근거가 될 사내 지식 확인.
- `explain_inquiry_behavior` — 시스템이 왜 그렇게 동작하는지 설명이 필요할 때.
- 고객·주문 배경이 필요하면 `txinquiry-customer-context` 스킬로 넘어간다.

## 2. 답변 작성

- `get_email_templates`, `list_template_categories` — 표준 문구가 있으면 먼저 확인해 재사용한다.
- 답변 초안은 **사용자에게 보여주고 동의를 받은 뒤** 등록한다. 임의로 고객에게 나가는 문구를
  확정하지 않는다.

## 3. 처리 (쓰기 — 전부 `confirm=true` 필요)

| 하려는 일 | 도구 | 주의 |
|---|---|---|
| 답변 등록 | `add_inquiry_reply` | **고객에게 발송되는 내용**이다. 문구를 그대로 보여주고 동의받는다 |
| 상태 변경 | `change_inquiry_status` | 완료·보류 전이는 담당 배정·답변 여부를 먼저 확인 |
| 고객 메일 재발송 | `resend_customer_email` | 중복 수신이 되므로 왜 다시 보내는지 확인받는다 |
| 메일 템플릿 저장 | `save_email_template` | 다른 상담원도 쓰는 공용 자산이다 |
| 신규 문의 생성 | `create_inquiry` | 중복 생성 방지를 위해 기존 문의를 먼저 검색한다 |
| 티켓 데이터 항목 수정 | `set_inquiry_data_field` | 무엇을 무엇으로 바꾸는지 값 단위로 보고 |

## 4. 확인

처리 후 `get_inquiry_detail` 로 재조회해 답변·상태가 실제로 반영됐는지 확인하고 보고한다.
발송 실패나 부분 실패를 성공으로 말하지 않는다.

## 규칙

- 고객에게 나가는 문구는 **사내 용어·내부 시스템 이름을 쓰지 않는다.**
- 답변에 내부 도구 이름이나 내부 처리 방식을 설명하지 않는다. 사용자가 알아야 할 업무 결과만 말한다.
- 도구가 없는 기능(첨부 편집 등)은 화면에서 처리하도록 안내하고 다른 도구로 우회하지 않는다.
