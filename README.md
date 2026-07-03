# AI Inbox Copilot

> Gmail 메일을 자동으로 분석하여 요약, 분류, 중요도 판단, 보안 위험 탐지, 영수증 정리, 대응 행동 추천까지 수행하고 — 매일 아침 **카카오톡으로 브리핑을 전송**하는 지능형 메일 관리 Harness

🎬 **데모 영상:** https://youtu.be/qgsDzQE8kWQ

---

## Overview

AI Inbox Copilot은 매일 쌓이는 Gmail 메일을 자동으로 분석하여 사용자가 중요한 메일, 위험 메일, 결제/영수증 메일을 빠르게 파악할 수 있도록 돕는 AI Agent 시스템입니다.

학교 공지, 업무 메일, 금융/투자 알림, 뉴스레터, 광고, 보안 알림 등이 섞여 있을 때 사용자가 직접 모든 메일을 열어보지 않아도 하루의 핵심 정보를 **1분 안에**, 그것도 메일함이 아니라 **매일 쓰는 메신저(카카오톡)에서** 확인할 수 있도록 설계했습니다.

Upstage 부트캠프에서 **Timely 플랫폼** 위에 구축했습니다.

---

## Core Goals

- 최근 24시간 내 읽지 않은 Gmail 메일 분석
- 최대 50건까지 처리하여 컨텍스트 초과 방지
- 피싱, 사칭, 악성 링크, 위험 첨부파일 탐지 (DKIM/SPF/DMARC 발신자 인증 포함)
- 메일별 핵심 요약 및 유형 분류
- 중요도 High / Medium / Low 판단
- 영수증 및 정기결제 메일 정리, 30일 누적 결제 금액 집계
- 사용자가 바로 실행할 수 있는 행동 추천
- 아침 메일 브리핑 리포트 생성 → 카카오톡 전송

---

## Key Features

- **Security Check**
  피싱, 사칭, 악성 링크, 위험 첨부파일 여부를 **가장 먼저** 분석합니다. Security Score(0~100)와 5단계 Risk Level로 판정하며, 위험 메일은 이후 분석에서 제외됩니다.
- **Mail Summary**
  메일 제목, 발신자, 수신 시각, 본문 핵심 내용을 3문장 이내로 요약하고 키워드·마감일을 추출합니다.
- **Mail Classification**
  학교, 업무, 금융·투자, 광고, 뉴스레터, 보안, 기타로 분류합니다. 결제 관련 메일은 결제·영수증·구독·환불 등으로 세분화합니다.
- **Payment Analysis**
  영수증, 결제 내역, 정기결제 메일을 정리하고 누적 결제 금액을 집계합니다.
- **Priority Analysis**
  긴급성, 마감 여부, 업무/학업 관련성을 기준으로 중요도를 판단합니다.
- **Action Recommendation**
  읽기, 보관, 삭제, 주의 필요, 즉시 조치 중 적절한 행동을 판단 근거와 함께 추천합니다.
- **Morning Mail Brief**
  하루 메일 분석 결과를 1~2페이지 브리핑으로 정리해 카카오톡으로 전송합니다.

---

## Agents

**Root Agent(Orchestrator)** 가 전체 흐름을 지휘하며, Agent 간 데이터는
`security_data → summary_data / classification_data → payment_data / priority_data` 로 명시적으로 전달됩니다.

| Agent | Role | Skills |
|---|---|---|
| Security Analyst Agent | 보안 위험 분석 | security-check |
| Inbox Manager Agent | 메일 수집, 요약, 분류 흐름 관리 | mail-summary, mail-classification |
| Payment Agent | 영수증 및 정기결제 정리 | payment-analysis |
| Priority Agent | 메일 중요도 판단 | priority-analysis |
| Briefing Agent | 최종 브리핑 및 행동 추천 | action-recommendation, morning-mail-brief |

---

## Workflow

```text
Trigger (매일 아침 자동 실행)
   │
   ▼
Gmail Connector — 최근 24시간 내 미열람 메일 수집 (최대 50건)
   │
   ▼
Security Check ─── 위험(HIGH↑) 메일은 이후 분석 제외, 보안 경고로 직행
   │
   ▼
Mail Summary
   │
   ▼
Mail Classification
   │
   ├──────────────┐
   ▼              ▼
Payment        Priority        ← 분류 결과를 받아 병렬 실행
Analysis       Analysis
   │              │
   └──────┬───────┘
          ▼
Action Recommendation
          │
          ▼
Morning Mail Brief
          │
          ▼
카카오톡 브리핑 전송
```

### 설계 의사결정: 보안 검사가 맨 앞인 이유

초기 설계는 요약이 1순위였습니다. 그런데 테스트 중 **피싱 메일이 '긴급', '오늘까지' 같은 표현 때문에 우선순위 최상위에 노출되는 결함**을 발견했습니다. 보안 검사를 원문 기준으로 파이프라인 맨 앞으로 옮겨, 위험 메일은 중요도 평가 자체를 받지 못하게 했습니다 — 오분류 원천 차단 + 걸러진 메일에 요약 비용도 쓰지 않습니다.

### Skill 고립 설계

일관된 브리핑 형식을 위해 Skill들을 거의 고립 수준으로 분리했습니다. 각 SKILL.md에 프롬프트·절차·출력 템플릿을 고정하고, Skill 간 통신은 "다음 Skill 전달 정보"로만 합니다. 매일 정해진 시각에 한 번 알려주는 시스템이므로, **속도 대신 출력 형식의 일관성과 정확도에 몰입**했습니다.

---

## Harness 구성 (6요소)

| 요소 | 정의 | 이 프로젝트에서 |
|---|---|---|
| Spec | 목표 · 범위 · 성공 기준 | 기획서 + skill별 spec.md |
| Skill | 반복작업 절차 카드 | SKILL.md 7개 |
| Subagent | 각 단계별 독립 실행 단위 | Root + 5개 Agent |
| Workflow | 실행 순서 · 분기 · 데이터 흐름 | 보안 선행 + 결제/우선순위 병렬 분기 |
| Hook | 자동 실행 트리거 | 매일 아침 자동 실행 + 예외 처리 규칙 |
| Checklist | 완료의 기준 | 정량 검증 6항목, 브리핑 전송 전 형식 점검 |

---

## Validation

라벨링된 테스트 메일 **20건**(정상 12 · 광고 4 · 피싱 모의 4)으로 측정했습니다.

| 항목 | 결과 |
|---|---|
| 피싱 탐지 | **4건 / 4건** (탐지율 100%) |
| 분류 정확도 | **100%** (오분류 0건) |
| 날짜·금액 원문 일치 | **100%** (환각 없음) |

결제 금액을 다루는 시스템이므로 숫자 정확성을 검증 1순위로 뒀습니다.

---

## Hook / 예외 처리 규칙

AI가 틀릴 수 있다는 전제로 설계했습니다.

| 상황 | 처리 규칙 |
|---|---|
| Gmail 연동 실패 | 재시도 1회 후 중단 — 빈 브리핑을 생성하지 않음 |
| 메일 0건 / 50건 초과 | '수집할 메일 없음' 명시 / 최신순 50건 + "N건 중 50건 분석" 표기 |
| 피싱 판정 | 자동 삭제하지 않음 — 차단 '권고'만, 최종 결정은 사용자 (Human-in-the-loop) |
| 본문 내 개인정보 | 계좌번호·주민번호 등 마스킹 처리 |
| 판단 불확실 | 확신 없는 '안전' 대신 '주의 필요'로 유보 + 사유 명시 |

> 실제 브리핑 하단에도 명시됩니다: *"삭제·구독해지·일정등록·답장전송 등은 자동 수행되지 않았으며, 모두 사용자 확인이 필요합니다."*

---

## 실행 환경 및 재현 방법

- **플랫폼:** Timely (강의·개발·자동화 실행 전부 Timely 위에서 진행)
- **연동:** Gmail Connector, 카카오톡 알림 채널
- **모델:** Claude Haiku 4.5

1. Timely에서 `agent/agent_prompt.md` 내용으로 Root Agent 생성
2. `skills/` 하위 7개 폴더의 SKILL.md · spec.md를 Timely Skill로 업로드
3. Gmail Connector와 카카오톡 알림 채널 연결
4. Agent별 자동화(반복작업)를 등록하고 실행 시각 설정

---

## Team

**부트캠프 5조 · Team 코듀** — 이유민, 어영욱, 김가빈, 김한진

© 2026 Team 코듀. Upstage X+AI·SW 부트캠프 프로젝트.
