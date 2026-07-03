---
name: morning-mail-brief
description: 모든 Skill의 분석 결과를 종합하여 사용자에게 최종 메일 브리핑을 제공하는 Skill이다.
---

# Morning Mail Brief

## 역할

Security Check, Mail Summary, Mail Classification,
Payment Analysis, Priority Analysis,
Action Recommendation 결과를 종합하여

사용자가 하루 메일을 1분 안에 파악할 수 있는
최종 브리핑을 생성한다.

이 Skill은 프로젝트의 **최종 출력(Output)** 을 담당한다.

---

## 입력

### Security Check 결과

- security_data

### Mail Summary 결과

- summary_data

### Mail Classification 결과

- classification_data

### Payment Analysis 결과

- payment_data

### Priority Analysis 결과

- priority_data

### Action Recommendation 결과

- recommendation_data

### 메타 정보

- 분석 시간
- 분석 기간
- 총 메일 수
- 분석된 메일 수

---

## 수행 절차

### 1. 메일 현황 생성

다음을 계산한다.

- 총 메일 수
- 분석된 메일 수
- 카테고리별 분포
- 중요도 분포

---

### 2. 긴급 메일 생성

추천 행동이

- 읽기
- 즉시 조치
- 주의 필요

인 메일을 우선 표시한다.

최대 5건만 출력한다.

---

### 3. 관심 메일 생성

Medium 중요도의

- 학교
- 업무
- 금융
- 관심사

메일을 정리한다.

---

### 4. 보안 경고 생성

Security Check 결과를 이용한다.

구분

- 🔴 CRITICAL
- 🔴 HIGH
- 🟡 MEDIUM
- 🟢 SAFE

판단 근거를 함께 표시한다.

---

### 5. 결제 요약 생성

Payment Analysis 결과를 이용한다.

다음을 표시한다.

- 최근 30일 결제 금액
- 구독 서비스
- 다음 결제 예정
- 이상 거래

---

### 6. 브리핑 생성

아래 순서를 반드시 따른다.

1. 메일 현황
2. 긴급·중요
3. 관심 메일
4. 보안 경고
5. 영수증

---

## 출력

최종 Markdown 문서를 생성한다.

파일명

```
MorningMailBrief_{YYYYMMDD}.md
```

---

## 출력 형식

````markdown
# 📧 Morning Mail Brief

생성 시간:

분석 기간:

---

# 📥 수신 메일

- 총 메일
- 카테고리 분포
- 중요도 분포

---

# 📌 긴급·중요

|제목|발신자|중요도|행동|
|---|---|---|---|

---

# 💼 관심 메일

...

---

# 🛡 보안 경고

## 🔴 HIGH

...

## 🟡 MEDIUM

...

## 🟢 SAFE

...

---

# 💳 영수증

- 최근 30일 총 지출
- 구독 서비스
- 다음 결제 예정

---

# ✅ 오늘 추천 행동

1.
2.
3.
