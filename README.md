# AI Inbox Copilot

> Gmail 메일을 자동으로 분석하여 요약, 분류, 중요도 판단, 보안 위험 탐지, 영수증 정리, 대응 행동 추천까지 수행하는 지능형 메일 관리 Harness

---

## Overview

AI Inbox Copilot은 매일 쌓이는 Gmail 메일을 자동으로 분석하여 사용자가 중요한 메일, 위험 메일, 결제/영수증 메일을 빠르게 파악할 수 있도록 돕는 AI Agent 시스템입니다.

학교 공지, 업무 메일, 금융/투자 알림, 뉴스레터, 광고, 보안 알림 등이 섞여 있을 때 사용자가 직접 모든 메일을 열어보지 않아도 하루의 핵심 정보를 1분 안에 확인할 수 있도록 설계했습니다.

---

## Core Goals

- 최근 24시간 내 읽지 않은 Gmail 메일 분석
- 최대 50건까지 처리하여 컨텍스트 초과 방지
- 피싱, 사칭, 악성 링크, 위험 첨부파일 탐지
- 메일별 핵심 요약 및 유형 분류
- 중요도 High / Medium / Low 판단
- 영수증 및 정기결제 메일 정리
- 사용자가 바로 실행할 수 있는 행동 추천
- 아침 메일 브리핑 리포트 생성

---

## Key Features

- Security Check  
  피싱, 사칭, 악성 링크, 위험 첨부파일 여부를 먼저 분석합니다.

- Mail Summary  
  메일 제목, 발신자, 수신 시각, 본문 핵심 내용을 3문장 이내로 요약합니다.

- Mail Classification  
  학교, 업무, 금융·투자, 광고, 뉴스레터, 보안, 기타로 분류합니다.

- Payment Analysis  
  영수증, 결제 내역, 정기결제 메일을 정리합니다.

- Priority Analysis  
  긴급성, 마감 여부, 업무/학업 관련성을 기준으로 중요도를 판단합니다.

- Action Recommendation  
  읽기, 보관, 삭제, 주의 필요, 즉시 조치 중 적절한 행동을 추천합니다.

- Morning Mail Brief  
  하루 메일 분석 결과를 1~2페이지 브리핑으로 정리합니다.

---

## Agents

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
User Trigger
   │
   ▼
Gmail Connector
   │
   ▼
Collect unread emails from last 24 hours
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
   ▼
Payment Analysis
   │
   ▼
Priority Analysis
   │
   ▼
Action Recommendation
   │
   ▼
Morning Mail Brief
   │
   ▼
User Report
