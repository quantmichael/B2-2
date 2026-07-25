# 뉴스 요약 자동화 시스템

## 프로젝트 소개

Google News RSS에서 최신 뉴스를 자동으로 수집한 후, AI 관련 뉴스만 선별하여 Google Gemini AI로 핵심 내용을 요약하고, 요약 결과를 Notion Database에 저장한 뒤 Discord 채널로 자동 전송하는 뉴스 자동화 시스템입니다.

또한 GUID를 이용한 중복 저장 방지 기능을 적용하여 동일한 뉴스가 여러 번 저장되지 않도록 구현하였습니다.

---

## 프로젝트 목표

- Google News RSS를 활용한 최신 뉴스 자동 수집
- AI 관련 뉴스 자동 필터링
- Google Gemini AI를 활용한 뉴스 요약
- Notion Database 자동 저장
- Discord 실시간 알림
- GUID 기반 중복 뉴스 저장 방지
- 반복 업무 자동화를 통한 업무 효율 향상

---

## 사용 기술

- Make
- Google News RSS
- Google Gemini AI
- Notion Database
- Discord Webhook

---

## 시스템 구성

```text
Google News RSS
        │
        ▼
RSS Trigger
        │
        ▼
Keyword Filter
        │
        ▼
Notion Search Objects
        │
        ▼
Duplicate Filter
        │
        ▼
Google Gemini AI
        │
        ▼
Notion Database
        │
        ▼
Discord Webhook
```

---

## 워크플로우

| 단계 | 설명 |
|------|------|
| Google News RSS | 최신 뉴스 자동 수집 |
| Keyword Filter | AI 관련 뉴스 필터링 |
| Notion Search Objects | GUID 기반 중복 뉴스 검색 |
| Duplicate Filter | 신규 기사만 통과 |
| Google Gemini AI | 뉴스 핵심 내용 요약 |
| Notion Database | 제목, 요약, 링크, 발행일, 출처, GUID 저장 |
| Discord Webhook | 요약 결과 자동 전송 |

---

## Notion Database 구성

| 필드 | 타입 |
|------|------|
| 제목 | Title |
| 요약 | Text |
| 링크 | URL |
| 발행일 | Date |
| 출처 | Text |
| GUID | Text |

GUID는 RSS에서 제공하는 고유 식별자를 저장하며, 동일한 뉴스의 중복 저장 여부를 확인하는 데 사용됩니다.

---

## 주요 기능

- Google News RSS 기반 뉴스 자동 수집
- AI 관련 뉴스 자동 필터링
- Google Gemini AI를 활용한 뉴스 요약
- Notion Database 자동 저장
- 뉴스 출처(Source) 자동 저장
- GUID 기반 중복 뉴스 저장 방지
- Discord 채널 자동 알림
- Make 기반 자동화 워크플로우

---

## 프로젝트 구조

```text
.
├── README.md
├── docs
└── images
```

---

## 설치 및 실행 방법

1. Make에서 새로운 Scenario를 생성합니다.
2. Google News RSS 모듈을 추가합니다.
3. AI 관련 뉴스만 통과하도록 Keyword Filter를 설정합니다.
4. Notion Database를 생성합니다.
5. Notion Search Objects를 추가합니다.
6. GUID를 기준으로 중복 여부를 검색합니다.
7. 검색 결과가 0건인 경우에만 Google Gemini AI를 실행합니다.
8. 요약 결과를 Notion Database에 저장합니다.
9. Discord Webhook으로 결과를 전송합니다.
10. Scheduler를 실행하여 정상 동작을 확인합니다.

---

## 실행 결과

- Google News RSS에서 최신 뉴스 자동 수집
- AI 관련 뉴스 자동 필터링
- Google Gemini AI를 통한 뉴스 요약
- Notion Database 자동 저장
- 뉴스 출처(Source) 저장
- GUID 기반 중복 저장 방지
- Discord 자동 알림 전송
- 전체 자동화 워크플로우 정상 동작 확인

---

## 향후 개선 사항

- 뉴스 카테고리별 자동 분류
- 사용자 키워드 설정 기능
- 이메일 알림 기능 추가
- 요약 품질 향상을 위한 프롬프트 개선
- 뉴스 통계 및 대시보드 기능 추가

---

## 팀원

| 이름 | 역할 |
|------|------|
| 남병광 | PM / GitHub 관리 |
| 정가영 | 기획 |
| 곽승렬 | 시스템 설계 |
| 정형경 | 구현 |
| 심동석 | 문서화 및 테스트 |
