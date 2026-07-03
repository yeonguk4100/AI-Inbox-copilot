---
name: priority-analysis
description: Mail Summary와 Mail Classification 결과를 기반으로 이메일의 중요도와 긴급도를 평가하여 Priority 정보를 생성하는 Skill이다.
---

# Priority Analysis

## 역할

Mail Summary와 Mail Classification 결과를 기반으로 이메일의 중요도와 긴급도를 평가한다.

이 Skill은 **중요도 분석만 수행**한다.

다음 작업은 수행하지 않는다.

- 메일 요약
- 메일 분류
- 보안 위험 분석
- 결제 정보 분석
- 행동 추천
- 최종 브리핑 생성

---

## 입력

### Mail Summary 결과

- summary
- keywords
- sender_name
- sender_email
- sender_domain
- received_time
- deadline
- attachment

### Mail Classification 결과

- category
- confidence

### 메일 원본 정보

- 메일 제목
- 메일 본문

---

## 수행 절차

### 1. 메일 정보 확인

다음을 확인한다.

- 메일 제목
- 한 줄 요약
- 주요 키워드
- 발신자
- 카테고리
- 마감일

---

### 2. 중요도 평가

다음 요소를 종합하여 중요도를 판단한다.

- 오늘 또는 24시간 이내 마감 여부
- 답장이 필요한 메일 여부
- 학교 관련 메일
- 업무 관련 메일
- 금융·투자 관련 메일
- 사용자 관심사
- 첨부파일 확인 필요 여부

중요도는 반드시 아래 중 하나를 선택한다.

- High
- Medium
- Low

판정 기준

#### High

- 오늘 또는 24시간 이내 마감
- 즉시 확인이 필요한 학교·업무 메일
- 금융·보안 관련 즉시 대응 메일
- 답장이 반드시 필요한 메일

#### Medium

- 3~7일 이내 마감
- 일반 학교 메일
- 일반 업무 메일
- 첨부파일 또는 일정 확인 필요

#### Low

- 광고
- 프로모션
- 일반 뉴스레터
- 단순 알림
- 이미 처리된 메일

---

### 3. 긴급도 평가

긴급도를 다음 중 하나로 판단한다.

- 높음
- 중간
- 낮음

---

### 4. 판단 근거 작성

다음 요소 중 하나 이상을 근거로 작성한다.

- 마감일
- 중요 발신자
- 카테고리
- 키워드
- 사용자 관심사
- 일정

근거 없는 판정은 하지 않는다.

---

### 5. 우선 확인 여부

다음을 결정한다.

- 즉시 확인 필요
- 나중에 확인 가능

---

## 출력

사용자에게 보여줄 Markdown 문서를 생성하지 않는다.

Markdown 테이블을 생성하지 않는다.

`.md` 파일을 생성하지 않는다.

Action Recommendation Skill이 사용할 수 있도록 구조화된 데이터만 반환한다.

출력은 아래 구조를 따른다.

```yaml
priority_data:

  - mail_id:
    priority:
    urgency:
    priority_score:
    priority_reason:
    should_check_now:
    reply_required:
    deadline:
    category:
    sender_name:
    sender_email:
```

---

## 출력 예시

```yaml
priority_data:

  - mail_id: "mail_001"

    priority: High

    urgency: 높음

    priority_score: 94

    priority_reason:
      - "오늘 마감"
      - "학교 공지"
      - "수강신청 일정"

    should_check_now: true

    reply_required: false

    deadline: "2026-07-03"

    category:
      - 학교

    sender_name: "강원대학교"

    sender_email: "notice@kangwon.ac.kr"
```

---

## 사용 규칙

- 중요도는 반드시 High, Medium, Low 중 하나만 선택한다.
- 긴급도는 반드시 높음, 중간, 낮음 중 하나만 선택한다.
- 객관적인 정보만 사용한다.
- 판단 근거를 반드시 제공한다.
- 여러 조건이 동시에 만족되면 가장 높은 중요도를 적용한다.
- 메일을 삭제하거나 이동하지 않는다.
- 추측하지 않는다.
- 불확실한 내용은 "확인 필요"로 표시한다.
- 사용자에게 보여줄 Markdown 문서를 생성하지 않는다.
- Markdown 테이블을 생성하지 않는다.
- `.md` 파일을 생성하지 않는다.
- Action Recommendation Skill이 사용할 구조화된 데이터만 반환한다.

---

## 출력 품질 기준

출력 결과는 다음 조건을 만족해야 한다.

- 모든 메일에 대해 중요도를 제공한다.
- 긴급도를 제공한다.
- 판단 근거를 제공한다.
- 우선 확인 여부를 제공한다.
- Action Recommendation이 바로 사용할 수 있는 구조화된 데이터를 반환한다.
- 역할 범위를 벗어난 작업을 수행하지 않는다.
