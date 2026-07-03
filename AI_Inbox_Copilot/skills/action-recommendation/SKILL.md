---
name: action-recommendation
description: Priority Analysis, Security Check, Mail Summary, Mail Classification, Payment Analysis 결과를 종합하여 사용자가 각 메일에 대해 수행해야 할 추천 행동을 생성하는 Skill이다.
---

# Action Recommendation

## 역할

Priority Analysis, Security Check, Mail Summary, Mail Classification, Payment Analysis 결과를 종합하여 각 메일에 대한 추천 행동을 결정한다.

이 Skill은 **추천 행동 데이터만 생성**한다.

다음 작업은 수행하지 않는다.

- 메일 요약
- 메일 분류
- 보안 위험 분석
- 중요도 판단
- 결제 정보 분석
- 최종 브리핑 생성

최종 출력 문서는 **Morning Mail Brief** Skill이 생성한다.

---

## 입력

### Priority Analysis 결과

- priority
- urgency
- priority_score
- priority_reason
- should_check_now
- reply_required
- deadline

### Security Check 결과

- security_score
- risk_level
- risk_reasons
- phishing_probability

### Mail Summary 결과

- summary
- keywords
- deadline
- attachment

### Mail Classification 결과

- category
- confidence

### Payment Analysis 결과

- payment_amount
- merchant
- payment_date
- is_receipt
- is_subscription
- abnormal_transaction

### 메일 기본 정보

- mail_id
- title
- sender_name
- sender_email
- received_time

---

## 수행 절차

### 1. 메일 분석 결과 확인

모든 Skill의 결과를 확인한다.

다음을 종합적으로 분석한다.

- 중요도
- 긴급도
- 보안 위험도
- 메일 분류
- 결제 정보
- 메일 요약

---

### 2. 추천 대상 선정

다음 조건을 만족하는 메일을 우선 선정한다.

- Priority가 High 또는 Medium
- Security Risk Level이 HIGH 또는 CRITICAL
- 결제 확인이 필요한 메일
- 답장이 필요한 메일
- 마감일이 임박한 메일

최대 5개의 메일만 추천 대상으로 선정한다.

---

### 3. 추천 행동 결정

각 메일에 대해 아래 행동 중 하나만 선택한다.

| 추천 행동 | 적용 기준 |
|-----------|-----------|
| 읽기 | 중요도가 높고 즉시 확인이 필요한 메일 |
| 보관 | 참고 가치가 있는 메일 |
| 삭제 | 광고, 중복, 중요하지 않은 메일 |
| 주의 필요 | 피싱, 사칭, 악성 링크, 위험 첨부파일 의심 |
| 즉시 조치 | 계정 탈취, 금융 피해, 긴급 대응 필요 |

---

### 4. 추천 근거 작성

추천 행동의 근거를 작성한다.

근거는 다음 요소 중 하나 이상을 포함한다.

- 중요도
- 긴급도
- 보안 위험도
- 메일 분류
- 결제 정보
- 마감일

---

### 5. 사용자 확인 여부 판단

다음 경우 반드시 사용자 확인이 필요하다.

- 추천 행동이 "주의 필요"
- 추천 행동이 "즉시 조치"
- Security Risk Level이 HIGH 또는 CRITICAL
- 결제 또는 금융 관련 메일
- 기한이 임박한 메일

그 외에는 확인이 필요하지 않다.

---

## 출력

사용자에게 보여줄 Markdown 문서를 생성하지 않는다.

Markdown 테이블을 생성하지 않는다.

`.md` 파일을 생성하지 않는다.

Morning Mail Brief가 사용할 수 있도록 구조화된 데이터만 반환한다.

출력은 아래 구조를 따른다.

```yaml
recommendation_data:

  - mail_id:
    title:
    sender_name:
    sender_email:
    received_time:

    category:

    priority:
    urgency:
    priority_score:

    security_score:
    risk_level:

    recommended_action:

    action_reason:

    confirmation_required:

    should_check_now:

    deadline:

    summary:

    payment_amount:

    abnormal_transaction:
```

---

## 출력 예시

```yaml
recommendation_data:

  - mail_id: "mail_001"

    title: "수강신청 일정 안내"

    sender_name: "강원대학교"

    sender_email: "notice@kangwon.ac.kr"

    received_time: "2026-07-02 09:00"

    category:
      - 학교

    priority: High

    urgency: 높음

    priority_score: 94

    security_score: 8

    risk_level: SAFE

    recommended_action: 읽기

    action_reason:
      - 오늘 마감되는 학사 일정
      - 중요도 High
      - 학교 공식 메일

    confirmation_required: No

    should_check_now: true

    deadline: "2026-07-03"

    summary: "수강신청 일정 및 유의사항 안내"

    payment_amount: 없음

    abnormal_transaction: 없음
```

---

## 사용 규칙

- 추천 행동은 반드시 하나만 선택한다.
- 실제 메일을 삭제하지 않는다.
- 실제 메일을 이동하지 않는다.
- 실제 답장을 보내지 않는다.
- 실제 캘린더를 등록하지 않는다.
- 실제 결제를 취소하지 않는다.
- 개인정보는 마스킹 처리한다.
- 날짜와 금액은 원문 그대로 사용한다.
- Security Check 결과를 변경하지 않는다.
- 사용자에게 보여줄 Markdown 문서를 생성하지 않는다.
- Markdown 테이블을 생성하지 않는다.
- `.md` 파일을 생성하지 않는다.
- Morning Mail Brief가 사용할 구조화된 데이터만 반환한다.

---

## 출력 품질 기준

출력 결과는 다음 조건을 만족해야 한다.

- 추천 행동을 정확히 하나 선택한다.
- 추천 근거를 제공한다.
- 사용자 확인 여부를 제공한다.
- Priority Analysis 결과를 반영한다.
- Security Check 결과를 반영한다.
- Payment Analysis 결과를 반영한다.
- Morning Mail Brief가 바로 사용할 수 있는 구조화된 데이터를 반환한다.
- 역할 범위를 벗어난 작업을 수행하지 않는다.
