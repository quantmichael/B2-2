# 뉴스 요약 자동화 시스템

## 프로젝트 소개

RSS에서 최신 뉴스를 자동으로 수집한 후 Google Gemini AI를 활용하여 핵심 내용을 요약하고, 요약 결과를 Notion 데이터베이스에 저장한 뒤 Discord 채널로 자동 전송하는 뉴스 자동화 시스템입니다.

---

## 프로젝트 목표

- RSS를 통한 최신 뉴스 자동 수집
- AI 기반 뉴스 요약 자동화
- Notion 데이터베이스 자동 저장
- Discord 실시간 알림 제공
- 반복 업무 자동화를 통한 업무 효율 향상

---

## 사용 기술

- Make
- RSS
- Google Gemini AI
- Notion
- Discord Webhook

---

## 시스템 구성

```text
RSS Feed
    │
    ▼
RSS Trigger
    │
    ▼
Keyword Filter
    │
    ▼
Google Gemini AI
    │
    ├────────► Notion Database
    │
    ▼
Discord Webhook
```

---

## 워크플로우

| 단계 | 설명 |
|------|------|
| RSS Trigger | 최신 뉴스 자동 수집 |
| Keyword Filter | 키워드 기반 뉴스 필터링 |
| Google Gemini AI | 뉴스 핵심 내용 요약 |
| Notion | 제목, 링크, 요약 자동 저장 |
| Discord | 요약 결과 자동 전송 |

---

## 주요 기능

- RSS 기반 최신 뉴스 자동 수집
- 키워드 기반 뉴스 필터링
- 중복 뉴스 자동 제거
- Google Gemini AI를 활용한 뉴스 요약
- Notion 데이터베이스 자동 저장
- Discord 채널 자동 알림
- Make 기반 자동화 워크플로우

---

## 프로젝트 구조

```text
📁 docs
📁 images
README.md
```

---

## 팀원

| 이름 | 역할 |
|------|------|
| 남병광 | PM / GitHub 관리 |
| 정가영 | 기획 |
| 곽승렬 | 시스템 설계 |
| 정형경 | 구현 |
| 심동석 | 문서화 및 테스트 |

---

## 실행 결과

- RSS에서 최신 뉴스 자동 수집
- 키워드 기반 뉴스 필터링
- 중복 뉴스 자동 제거
- Google Gemini AI를 활용한 뉴스 요약
- Notion 데이터베이스 자동 저장
- Discord 채널 자동 알림
- 전체 자동화 워크플로우 정상 동작 확인

---

## 향후 개선 사항

---
