# Workflow

```text
                  AI Inbox Copilot

                         │
                         ▼
                 📥 새 메일 수신
                         │
                         ▼
        ┌──────────────────────────┐
        │ Inbox Manager Agent      │
        │ Gmail Connector          │
        └──────────────────────────┘
                         │
                         ▼
        ┌──────────────────────────┐
        │ Security Check           │
        └──────────────────────────┘
                         │
             안전한 메일만 다음 단계 진행
                         │
                         ▼
        ┌──────────────────────────┐
        │ Mail Summary             │
        └──────────────────────────┘
                         │
                         ▼
        ┌──────────────────────────┐
        │ Mail Classification      │
        └──────────────────────────┘
                    │          │
        ┌───────────┘          └───────────┐
        ▼                                  ▼
┌────────────────────┐            ┌────────────────────┐
│ Payment Analysis   │            │ Priority Analysis  │
└────────────────────┘            └────────────────────┘
        └───────────────┬──────────────────────────────┘
                        ▼
        ┌──────────────────────────┐
        │ Action Recommendation    │
        └──────────────────────────┘
                        │
                        ▼
        ┌──────────────────────────┐
        │ Morning Mail Brief       │
        └──────────────────────────┘
                        │
                        ▼
              👤 사용자에게 결과 제공
```

---

## Workflow

### 1. 새 메일 수신

Inbox Manager Agent가 Gmail Connector를 통해 최근 24시간 내 읽지 않은 메일(최대 50건)을 수집한다.

---

### 2. Security Check

Security Analyst Agent가 모든 메일의 보안 위험도를 분석한다.

- 발신자 도메인 검증
- 피싱 및 사칭 여부 확인
- 악성 링크 분석
- 위험 첨부파일 확인

위험 여부를 판단하고 분석 결과를 생성한다.

---

### 3. Mail Summary

보안 검사를 통과한 메일의 핵심 내용을 요약한다.

- 한 줄 요약
- 핵심 내용
- 주요 키워드
- 일정 및 마감일
- 첨부파일 정보

---

### 4. Mail Classification

메일을 분석하여 적절한 카테고리로 분류한다.

- 학교
- 업무
- 금융·투자
- 광고
- 뉴스레터
- 보안
- 기타

또한 Payment Analysis와 Priority Analysis가 사용할 정보를 함께 생성한다.

---

### 5. Payment Analysis

영수증 및 결제 관련 메일을 분석한다.

- 영수증 여부
- 정기결제 여부
- 결제 금액
- 월간 결제 내역

---

### 6. Priority Analysis

메일의 중요도를 분석한다.

다음을 기준으로 High / Medium / Low를 결정한다.

- 긴급성
- 업무 관련성
- 학사 일정
- 마감일

---

### 7. Action Recommendation

모든 분석 결과를 종합하여 사용자에게 적절한 행동을 추천한다.

예시

- 읽기
- 보관
- 삭제
- 주의 필요
- 즉시 확인

---

### 8. Morning Mail Brief

최종 브리핑을 생성한다.

브리핑에는 다음 정보를 포함한다.

- 오늘 받은 메일 현황
- 긴급·중요 메일
- 관심사 및 업무 메일
- 보안 경고
- 영수증 및 결제 요약

---

## Pipeline

```
새 메일 수신
        │
        ▼
Security Check
        │
        ▼
Mail Summary
        │
        ▼
Mail Classification
        │
 ┌──────┴──────┐
 ▼             ▼
Payment     Priority
Analysis    Analysis
 └──────┬──────┘
        ▼
Action Recommendation
        ▼
Morning Mail Brief
        ▼
사용자
```
