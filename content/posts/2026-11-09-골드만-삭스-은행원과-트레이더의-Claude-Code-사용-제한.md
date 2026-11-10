---
title: "골드만 삭스, 은행원과 트레이더의 Claude Code 사용 제한"
date: "2026-11-09"
description: "Goldman Sachs는 10월 내부 지침을 개정해 Claude Code와 GitHub 사용을 엔지니어(및 Strats)로 제한했고, 대부분의 트레이더와 은행원은 업무에서 쓸 수 없게 됐다. 사내 다중 모델 AI 비서는 계속 전사 제공되며, Anthropic과의 협력과 IPO 공동 주관도 이어진다."
category: "기술경영·산업"
tags: ["Goldman-Sachs", "Claude-Code", "코딩-에이전트", "바이브-코딩", "정보-차단", "AI-거버넌스"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36827&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://www.thestateofai.com/news/goldman-sachs-claude-code-traders-bankers"
coverage: "full"
---

> 📌 2026-11-09 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36827&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://www.thestateofai.com/news/goldman-sachs-claude-code-traders-bankers)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Goldman Sachs는 10월 내부 지침을 개정해 Claude Code와 GitHub 사용을 엔지니어(및 Strats)로 제한했고, 대부분의 트레이더와 은행원은 업무에서 쓸 수 없게 됐다. 사내 다중 모델 AI 비서는 계속 전사 제공되며, Anthropic과의 협력과 IPO 공동 주관도 이어진다.

## 핵심

- Bloomberg 보도에 따르면 Goldman은 생산성·위험 우려를 검토한 뒤 10월 지침으로 Claude Code와 GitHub를 엔지니어로 제한했다. 거래·정량 연구·소프트웨어 엔지니어링을 넘나드는 Strats는 계속 접근할 수 있다.
- 제한 대상은 스스로 코드를 작성·실행하는 외부(third-party) 에이전트이고, 여러 회사 모델을 쓰는 사내 AI 비서는 계속 전사적으로 제공된다. 트레이더들은 고객 거래 흐름 모니터링, 시장·포지션 모델링, 문서·그래픽 자료 작성에 이 도구를 써 왔고 일부는 불만을 느낀다.
- CIO Marco Argenti는 7월까지 비기술 직원의 바이브 코딩을 긍정 평가했다. 트레이더와 은행원이 만든 도구를 약 1만 2,000명 엔지니어가 개선하는 방식이었으나, 이는 엔지니어링 교육이 없는 수천 명이 고객 정보를 다루는 컴퓨터에서 코드 실행 소프트웨어를 쓴다는 뜻이기도 했다.
- 원문이 꼽는 위험은 세 가지다. 에이전트 생성 코드가 검토·테스트·배포 통제 없이 일상 업무에 쓰일 수 있다는 점, 부서 간 정보 차단(information barrier)을 고려하지 않을 수 있다는 점, 작성자가 코드의 예외 처리를 이해하지 못해 손실로 이어질 수 있다는 점이다. 토큰 과금에 따른 비용도 영향을 줬을 수 있으나 확인된 원인은 아니다.
- Goldman은 4월 홍콩 은행원용 내부 AI 플랫폼에서 Claude를 제거했다(Anthropic 계약의 엄격한 해석, FT 보도). 이는 지역·계약 기반 제한이고 이번은 직무 기반 제한이다. JPMorgan도 6월 홍콩 승인 목록에서 Claude를 뺐다.
- Anthropic과의 관계는 유지된다. Anthropic 엔지니어들이 6개월간 상주해 거래 회계·고객 온보딩용 Claude 에이전트를 구축했고(백오피스, Goldman 엔지니어링팀 운영), 10월 26일 S-1에 따르면 Goldman과 Morgan Stanley가 Anthropic IPO 대표 주관사다. Nasdaq 티커 ANT로 11월 말 이전 상장 예상.
- State of AI의 논평: 제한은 타당하나 실행 방식이 문제다. 기업은 에이전트 생성 코드의 책임 엔지니어를 지정하고, 정보 차단·데이터 접근 권한·소프트웨어 승인 규칙을 동일하게 적용하며, 회수하는 도구의 기존 활용 업무를 기록해야 한다고 주장한다.

## 자세히

Claude Code는 파일을 읽고 프로그램을 작성해 실행하며 보통 사용자의 접근 권한을 그대로 쓴다. 엔지니어가 만든 코드는 검토·테스트·배포 통제를 거치지만, 트레이더가 고객 흐름을 보려고 만든 스크립트는 그런 절차 없이 쓰일 수 있다. Goldman의 내부 AI 플랫폼은 정보 차단 같은 통제를 중심으로 설계됐지만, 에이전트로 만든 도구는 이를 고려하지 않을 수 있고 엔지니어는 전달받은 도구만 점검할 수 있다.

다른 금융사 사례도 소개된다. Barclays는 10월 연말까지 개발자 절반이 Claude Code를 쓸 것이라고 밝힌 뒤 3주 후 5,000명 감원 대상에 개발자를 포함했다. JPMorgan과 Schwab은 이번 가을 외부 에이전트의 고객 계좌 접근을 차단했다. 비용 측면에서는 Amazon이 무의미한 작업으로 순위를 올리려는 직원들 때문에 내부 순위표를 폐지했고, Accenture는 직원당 월 29달러까지 Claude 비용을 지원한다.

이 글은 Bloomberg 보도를 2차로 정리한 State of AI의 기사이며, 후반부 "Our take"는 매체의 의견이다. Bloomberg 보도에는 제한 이후의 대체 수단이 언급되지 않았다고 한다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 에이전트 생성 코드의 책임 소재와 기존 통제(정보 차단·데이터 권한·승인 규칙) 적용이라는 거버넌스 논점이 직접 맞닿는다. 또 비엔지니어가 만든 도구를 엔지니어가 개선하는 조직 내 혁신 확산 방식과 그 회수 사례는 기술경영 연구의 소재가 될 수 있다.
