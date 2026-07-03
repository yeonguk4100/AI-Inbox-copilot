---
name: payment-analysis
description: 결제·영수증·구독·환불·자동결제 관련 메일을 분석하여 결제 정보를 구조화하는 Skill이다.
---

# Payment Analysis

## 역할

Mail Summary, Mail Classification, Security Check 결과를 기반으로 결제·영수증·구독·환불·자동결제 관련 메일을 분석한다.

이 Skill은 **결제 정보 분석만 수행**한다.

다음 작업은 수행하지 않는다.

- 메일 요약
- 메일 분류
- 보안 위험 분석
- 중요도 판단
- 행동 추천
- 최종 브리핑 생성

---

## 입력

### Mail Summary 결과

- summary
- keywords
- received_time
- deadline

### Mail Classification 결과

- category
- is_receipt
- is_subscription
- payment_amount

### Security Check 결과

- security_score
- risk_level
- phishing_probability

### 메일 원본 정보

- 메일 제목
- 메일 본문
- 발신자 이름
- 발신자 이메일

---

## 수행 절차

### 1. 결제 메일 선별

다음 조건 중 하나 이상을 만족하는 메일을 대상으로 한다.

- category에 금융·투자가 포함
- 영수증 메일
- 정기결제 메일
- 환불 메일
- 결제 승인 메일

---

### 2. 결제 정보 추출

각 메일에서 다음 정보를 추출한다.

- 결제 금액
- 통화
- 가맹점
- 결제일
- 결제수단
- 환불 여부
- 정기결제 여부
- 다음 결제 예정일

---

### 3. 최근 30일 결제 분석

최근 30일 기준으로

- 누적 결제 금액
- 환불 금액
- 통화별 합계

를 계산한다.

---

### 4. 지출 분석

- 지출 금액 기준으로 정렬한다.
- 상위 지출 항목을 생성한다.

---

### 5. 구독 서비스 분석

정기결제 메일을 기반으로

- 구독 서비스
- 월 결제 금액
- 다음 결제 예정일

을 정리한다.

---

### 6. 비정상 거래 확인

다음을 확인한다.

- Security Check 결과가 HIGH 이상
- 피싱 가능성이 높은 결제 메일
- 금액 확인 불가
- 결제 예정일 확인 불가

애매한 경우 "확인 필요"로 표시한다.

---

## 출력

사용자에게 보여줄 Markdown 문서를 생성하지 않는다.

Markdown 테이블을 생성하지 않는다.

`.md` 파일을 생성하지 않는다.

다음 Agent가 사용할 수 있도록 구조화된 데이터만 반환한다.

출력은 아래 구조를 따른다.

```yaml
payment_data:

  - mail_id:
    merchant:
    payment_amount:
    currency:
    payment_date:
    payment_method:
    is_receipt:
    is_subscription:
    refund:
    next_payment_date:
    total_payment_30d:
    top_spending:
    subscription_review_required:
    abnormal_transaction:
    security_score:
    risk_level:
```

---

## 출력 예시

```yaml
payment_data:

  - mail_id: "mail_001"

    merchant: "Netflix"

    payment_amount: "17,000원"

    currency: "KRW"

    payment_date: "2026-07-01"

    payment_method: "신용카드"

    is_receipt: 예

    is_subscription: 예

    refund: 아니오

    next_payment_date: "2026-08-01"

    total_payment_30d: "125,000원"

    top_spending:
      - Netflix
      - Google One
      - Coupang

    subscription_review_required: 아니오

    abnormal_transaction: 없음

    security_score: 8

    risk_level: SAFE
```

---

## 사용 규칙

- 결제 승인, 결제 취소, 환불 요청, 구독 해지, 카드 정지를 자동 수행하지 않는다.
- 모든 행동은 추천만 제공한다.
- 카드번호와 계좌번호는 반드시 마스킹한다.
- 금액과 날짜를 확인할 수 없으면 "확인 필요"로 표시한다.
- Security Check 결과가 HIGH 이상이면 결제 정보를 신뢰하지 않도록 표시한다.
- 공식 카드사 및 서비스에서 직접 확인하도록 안내한다.
- 사용자에게 보여줄 Markdown 문서를 생성하지 않는다.
- Markdown 테이블을 생성하지 않는다.
- `.md` 파일을 생성하지 않는다.
- 다음 Agent가 사용할 구조화된 데이터만 반환한다.

---

## 출력 품질 기준

출력 결과는 다음 조건을 만족해야 한다.

- 결제 관련 메일만 분석한다.
- 최근 30일 결제 정보를 계산한다.
- 구독 서비스를 식별한다.
- 비정상 거래 여부를 판단한다.
- Security Check 결과를 반영한다.
- 다음 Agent가 바로 사용할 수 있는 구조화된 데이터를 반환한다.
- 역할 범위를 벗어난 작업을 수행하지 않는다.
