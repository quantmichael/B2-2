# 프로젝트 변경 사항 적용 가이드

---

## 목적

이번 과제에서는 기존 시나리오를 개선하여 다음 기능을 추가하였습니다.

1. 연합뉴스 RSS → Google News RSS 변경
2. Google Sheets → Notion Database 변경
3. 뉴스 출처(Source) 저장 기능 추가
4. 동일 뉴스 중복 저장 방지 기능 추가

팀원들은 아래 내용을 순서대로 적용한 후 정상 동작 여부를 확인해 주시기 바랍니다.

---

# 1. Google News RSS로 변경

### 기존

연합뉴스 RSS 사용

### 변경

Google News RSS 사용

예시

```
https://news.google.com/rss?hl=ko&gl=KR&ceid=KR:ko
```

### 변경 이유

- 다양한 언론사의 AI 뉴스를 수집하기 위함
- Google News에서 제공하는 출처 정보를 활용하기 위함

---

# 2. Notion Database 사용

기존 Google Sheets 대신 Notion Database를 사용합니다.

## Database 속성

| 속성 | 타입 |
| --- | --- |
| 제목 | Title |
| 요약 | Text |
| 링크 | URL |
| 발행일 | Date |
| 출처 | Text |
| GUID | Text |

※ GUID는 RSS에서 전달되는 id 값을 저장하는 용도입니다.

---

# 3. 출처(Source) 저장

Google News RSS에서는 기사의 언론사 정보가 함께 제공됩니다.

예시

- 연합뉴스
- 뉴시스
- 전자신문
- ZDNet Korea
- 한국경제

RSS의 Source 값을 Notion의 **출처** 필드에 저장하도록 설정합니다.

---

# 4. 중복 저장 방지 기능

## 기존 문제

Scheduler가 실행될 때마다 동일한 뉴스가 계속 저장되는 문제가 있었습니다.

---

## 해결 방법

Gemini 실행 전에 Notion Search Objects를 추가합니다.

워크플로우

```
Scheduler
        ↓
Google News RSS
        ↓
AI Filter
        ↓
Notion Search Objects
        ↓
Filter
        ↓
Gemini AI
        ↓
Notion Database
        ↓
Discord
```

---

# 5. Notion Search Objects 설정

Object Type

```
Data Source Items
```

Data Source

```
사용 중인 Notion Database 선택
```

Filter

```
Property
GUID

Equals

RSS → guid
```

Limit

```
1
```

---

# 6. Search Objects 뒤 Filter 설정

Search Objects와 Gemini 사이에 Filter를 추가합니다.

조건

```
Total number of bundles

Equal to

0
```

의미

검색 결과가 없을 경우에만

Gemini AI가 실행됩니다.

이미 저장된 뉴스는 여기서 종료됩니다.

---

# 7. 전체 처리 흐름

```
RSS 수집
        ↓
AI 뉴스만 통과
        ↓
Notion에서 동일 기사 검색
        ↓
검색 결과 없음
        ↓
Gemini 요약
        ↓
Notion 저장
        ↓
Discord 알림
```

---

# 테스트 방법

### 테스트 1

새로운 기사가 들어왔을 때

기대 결과

- Gemini 실행
- Notion 저장
- Discord 전송

---

### 테스트 2

같은 기사를 다시 실행

기대 결과

- Gemini 실행 안 됨
- Notion 저장 안 됨
- Discord 전송 안 됨

---

### 테스트 3

Google News RSS에서 다른 언론사 기사 확인

기대 결과

Notion의 **출처** 필드에

예시

```
연합뉴스
뉴시스
전자신문
ZDNet Korea
```

등이 정상적으로 저장되는지 확인합니다.

---

# 확인 요청 사항

적용 후 아래 항목을 확인해 주세요.

- Google News RSS가 정상적으로 수집되는가?
- AI 뉴스 필터가 정상 동작하는가?
- Gemini 요약이 정상 생성되는가?
- Notion에 제목, 요약, 링크, 발행일, 출처, ID가 모두 저장되는가?
- 동일 뉴스가 중복 저장되지 않는가?
- Discord 알림이 정상 전송되는가?

# MAKE 워크플로우

![MAKE 워크플로우](./image.png)

**문제가 발생하거나 개선이 필요한 부분이 있으면 해당 단계와 오류 내용을 함께 공유해 주시면 검토 후 수정하겠습니다. 하다가 잘 안되시거나 궁금한 사항 있으시면 메시지 또는 평일 오전에 2층 창가(3-2-1-?) 자리로 와주세요** 

---

좋은 피드백입니다. 기존 가이드를 크게 수정할 필요는 없고, Parse JSON 설명 바로 아래에 아래 내용을 추가하면 초보자들이 가장 많이 헷갈리는 부분을 예방할 수 있습니다. GUIDE.md

⸻

# 8. Parse JSON 연결 시 주의사항 (추가)

Gemini Result를 직접 연결하지 마세요.

Gemini AI는 다음과 같은 JSON 문자열을 반환합니다.

{
  "summary": "AI 뉴스입니다.",
  "sentiment": "긍정"
}

이 상태에서 Gemini의 Result를 Notion이나 Discord에 직접 연결하면 JSON 원문이 그대로 출력됩니다.

예시

{
  "summary": "AI 뉴스입니다.",
  "sentiment": "긍정"
}

올바른 연결 방법

Gemini 결과를 Parse JSON 모듈로 전달하면 다음과 같이 각각의 데이터가 생성됩니다.

* Summary
* Sentiment

이후 Notion과 Discord에는 반드시 Parse JSON에서 생성된 Summary와 Sentiment 필드를 연결해야 합니다.

Discord 출력 예시

🤖 AI 요약
AI 뉴스입니다.
😄 감성
긍정

반드시 기억하세요.

Gemini Result → 직접 연결 (X)

Gemini Result
        ↓
Discord / Notion

→ JSON 문자열이 그대로 출력될 수 있습니다.

Parse JSON → Summary / Sentiment 연결 (O)

Gemini Result
        ↓
Parse JSON
        ↓
Summary
Sentiment
        ↓
Discord / Notion

→ 원하는 형태로 깔끔하게 출력됩니다.

많이 헷갈리는 부분입니다.
Discord와 Notion에는 Gemini의 Result가 아니라 Parse JSON에서 생성된 Summary와 Sentiment를 연결해야 합니다.

⸻
