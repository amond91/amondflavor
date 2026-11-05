---
title: "OpenAI가 Codex 대화의 모델 학습 사용을 알림 없이 다시 활성화하는 것으로 보임"
date: "2026-11-04"
description: "한 이용자와 동료들이 꺼 두었던 Codex(ChatGPT)의 모델 학습용 데이터 공유 설정이 알림 없이 다시 켜져 있었다고 Hacker News에 보고했다. 일부에 국한된 문제인지 광범위한 현상인지는 확인되지 않았고, 댓글에서는 다른 사례와 신뢰 문제를 두고 논쟁이 이어졌다."
category: "기술경영·산업"
tags: ["OpenAI", "Codex", "데이터-공유-설정", "모델-학습", "지식재산-유출", "다크-패턴"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36540&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://news.ycombinator.com/item?id=50352757"
coverage: "partial"
---

> 📌 2026-11-04 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36540&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://news.ycombinator.com/item?id=50352757)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

한 이용자와 동료들이 꺼 두었던 Codex(ChatGPT)의 모델 학습용 데이터 공유 설정이 알림 없이 다시 켜져 있었다고 Hacker News에 보고했다. 일부에 국한된 문제인지 광범위한 현상인지는 확인되지 않았고, 댓글에서는 다른 사례와 신뢰 문제를 두고 논쟁이 이어졌다.

## 핵심

- 게시자는 회사 지식재산 유출을 우려해 Codex/Claude Code 등의 데이터 공유를 꺼 두었는데, 동료들과 함께 Codex 설정이 조용히 다시 켜진 것을 발견했다고 한다. 누구도 의식적으로 다시 동의한 적이 없다고 밝혔다.
- 설정은 chatgpt.com/settings/data-controls 의 "Improve the model for everyone" 항목에서 확인할 수 있고, privacy.openai.com/policies?modal=take-control 페이지에서도 데이터 공유를 끌 수 있다고 한다. 게시자는 이전 HN 논의(id=49643556)에서도 비슷한 보고가 있었다고 덧붙였다.
- 한 댓글 작성자(chriskanan)는 자신도 설정이 다시 켜져 있었고, 과거에 학습 제외 요청 2건이 승인된 이력이 있는데도 재활성화됐다고 적었다. 반면 다른 댓글 작성자는 본인과 주변은 겪지 못했다며 입증을 요구했고, 게시자는 입증할 수 없으며 비난이 아니라 관찰이라고 답했다.
- Mistral을 쓰던 한 댓글 작성자는 opt-in에서 opt-out으로 정책이 바뀌고 조직 단위 설정이 사라지는 과정에서 직원 전원의 슬라이더가 켜짐으로 돌아갔다고 했다. 조직 단위 설정은 약 48시간 뒤 복원됐고, 학습에 데이터를 쓰지 않는다는 답변을 받았다고 한다.
- 한 댓글 작성자(AlanYx)는 Anthropic과 Microsoft(Azure)에서 대화가 반복 플래그되면 계약상 기밀 보호가 적용되지 않을 수 있다고 주장하며 OpenAI도 비슷할 것이라 추측했다. 이는 검증되지 않은 개인 의견이다. FTC가 동의 없는 또는 변경된 동의에 대해 경고해 왔다는 언급도 있다.
- Gemini에서 채팅 저장(Keep activity)이 AI 학습용 공유와 묶여 있다는 지적(다크 패턴)이 나왔다. 일부는 이 때문에 Gemini 대신 Anthropic을 택했다고 했고, 기업 계약은 소비자 계정과 다르다는 반론도 있었다.
- "진짜 신경 쓰면 자체 추론을 돌려야 한다"는 의견과, 소형 오픈웨이트 모델이면 충분하다는 반론도 있었다. 그 밖에 Sam Altman을 둘러싼 법적 논란을 두고 긴 곁가지 논쟁이 벌어졌다.

## 자세히

원문(HN 게시글)은 518 points, 댓글 154개로 표시되어 있다. 핵심 주장은 게시자의 단일 관찰(본인과 동료 몇 명)에 기반하며, OpenAI의 공식 설명이나 원인 분석은 원문에 없다. 게시자도 "고립된 문제인지 더 광범위한 것인지" 묻는 상태였다.

댓글은 크게 세 갈래다. 첫째, 유사 사례(chriskanan의 재활성화, Mistral의 정책 변경 중 전체 켜짐)와 다크 패턴 비판(Gemini). 둘째, 법적·계약적 보호의 실효성(FTC 경고, 집단소송 포기 조항, 플래그된 대화의 보관·학습 가능성)에 대한 추측. 셋째, 자체 호스팅 또는 오픈웨이트 모델 사용 같은 대안 논의다. 이 가운데 상당수는 근거 없는 추측이거나 개인 의견이다.

원문 마지막 댓글은 중간에 잘려 있다.

## 왜 골랐나

AI 에이전트 개발 시 Codex·Claude Code 같은 코딩 에이전트에 넣는 프롬프트가 학습에 쓰일 수 있다는 점은 연구·코드 지식재산 관리와 맞닿는다. 학습 제외 설정이 이용자 모르게 바뀔 수 있다면 주기적 점검이 필요하다는 시사점이 있다. 다만 이는 미확인 보고 한 건에 근거한다.
