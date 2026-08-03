# 보너스 과제 : AI 뉴스 감성 분석 기능 추가

기존 프로젝트에서는 Gemini AI를 이용하여 뉴스의 핵심 내용을
요약했습니다.

보너스 과제에서는 AI가 뉴스를 요약하는 것뿐만 아니라 **뉴스의 감성(긍정,
중립, 부정)**까지 분석하여 Notion과 Discord에 함께 저장하도록 기능을
확장합니다.

------------------------------------------------------------------------

## 1. Notion 데이터베이스 수정

기존 Notion 데이터베이스에 **감성** 속성을 추가합니다.

| 속성명 | 타입 |
| :------ | :---- |
| 감성 | Select |

Select 옵션은 다음과 같습니다.

- 긍정
- 중립
- 부정

## 2. Gemini 프롬프트 수정

``` text
다음 뉴스를 분석하세요.

1. 뉴스 핵심 내용을 3줄 이내로 요약하세요.
2. 뉴스의 감성을 "긍정", "중립", "부정" 중 하나로 분류하세요.
3. 반드시 아래 JSON 형식으로만 출력하세요.
4. 코드 블록(```)을 사용하지 마세요.
5. JSON 이외의 설명이나 문장은 절대 출력하지 마세요.

{
  "summary": "뉴스 요약",
  "sentiment": "<긍정|중립|부정>"
}

제목:
{{2.rssFields.title}}

내용:
{{2.rssFields.description}}
```

## 3. JSON Parse 모듈 추가

Gemini AI ↓ Parse JSON

JSON String : Gemini Result

Data Structure 예시

``` json
{
  "summary": "뉴스 요약",
  "sentiment": "긍정"
}
```

생성되는 필드

-   summary
-   sentiment

## 4. Notion 저장

Notion Create Database Item 모듈에서 아래와 같이 매핑합니다.

| Notion 속성 | Make 매핑 |
| :---------- | :-------- |
| 요약 | Parse JSON → summary |
| 감성 | Parse JSON → sentiment |

## 5. Discord 메시지

``` text
📰 AI 뉴스

📌 제목
{{2.title}}

😐 감성: {{23.sentiment}}

🤖 AI 요약
{{23.summary}}

🔗 원문
{{2.url}}
```

## 6. MAKE 워크플로우

![MAKE 워크플로우](../images/make_workflow.png)

## 7. 실행 결과

-   AI 뉴스 요약
-   AI 뉴스 감성 분석
-   Notion에 감성 정보 저장
-   Discord에 감성 정보 표시

## **8. Parse JSON 연결 시 주의사항 (추가)**

### **Gemini Result를 직접 연결하지 마세요.**

Gemini AI는 다음과 같은 JSON 문자열을 반환합니다.

```json
{
  "summary": "AI 뉴스입니다.",
  "sentiment": "긍정"
}
```

이 상태에서 **Gemini의 Result를 Notion이나 Discord에 직접 연결하면 JSON 원문이 그대로 출력됩니다.**

예시

```
{
  "summary": "AI 뉴스입니다.",
  "sentiment": "긍정"
}
```

### **올바른 연결 방법**

Gemini 결과를 **Parse JSON** 모듈로 전달하면 다음과 같이 각각의 데이터가 생성됩니다.

- Summary
- Sentiment

이후 **Notion과 Discord에는 반드시 Parse JSON에서 생성된 Summary와 Sentiment 필드를 연결**해야 합니다.

### **Discord 출력 예시**

```
🤖 AI 요약
AI 뉴스입니다.

😄 감성
긍정
```

### **반드시 기억하세요.**

**Gemini Result → 직접 연결 (X)**

```
Gemini Result
        ↓
Discord / Notion
```

→ JSON 문자열이 그대로 출력될 수 있습니다.

**Parse JSON → Summary / Sentiment 연결 (O)**

```
Gemini Result
        ↓
Parse JSON
        ↓
Summary
Sentiment
        ↓
Discord / Notion
```

→ 원하는 형태로 깔끔하게 출력됩니다.

**가장 많이 헷갈리는 부분입니다.**
Discord와 Notion에는 **Gemini의 Result가 아니라 Parse JSON에서 생성된 Summary와 Sentiment를 연결**해야 합니다.

---
