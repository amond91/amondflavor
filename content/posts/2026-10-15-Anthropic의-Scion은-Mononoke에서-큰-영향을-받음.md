---
title: "Anthropic의 Scion은 Mononoke에서 큰 영향을 받음"
date: "2026-10-15"
description: "Mononoke(Meta의 소스 제어 서버) 개발에 참여한 sunshowers 는 Anthropic 의 새 버전 관리 시스템 Scion 의 '파생 데이터(derived data)' 개념이 자신이 2022년 소개한 Mononoke 의 설계와 크게 겹치는데도 Mononoke 를 언급하지 않은 것은 표절에 가깝다고 비판합니다. 설계 복제 자체는 문제 삼지 않으며, 출처 인정이 핵심이라고 주장합니다."
category: "AI 에이전트·모델"
tags: ["Scion", "Mononoke", "Sapling", "파생-데이터", "출처-인정", "LLM-설계-보조"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35369"
original: "https://sunshowers.io/posts/anthropic-mononoke/"
coverage: "full"
---

> 📌 2026-10-15 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35369) · [원문 기사](https://sunshowers.io/posts/anthropic-mononoke/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Mononoke(Meta의 소스 제어 서버) 개발에 참여한 sunshowers 는 Anthropic 의 새 버전 관리 시스템 Scion 의 '파생 데이터(derived data)' 개념이 자신이 2022년 소개한 Mononoke 의 설계와 크게 겹치는데도 Mononoke 를 언급하지 않은 것은 표절에 가깝다고 비판합니다. 설계 복제 자체는 문제 삼지 않으며, 출처 인정이 핵심이라고 주장합니다.

## 핵심

- Anthropic 은 Scion 소개글에서 파생 데이터로 blame 같은 작업을 효율화하는 방식을 설명했고, 필자는 2022년 Oxide 지원 자료에서 Mononoke 가 같은 개념을 같은 용도로 쓴다고 이미 설명했다고 함. 필자는 이 유사성이 비슷한 통찰에 독립적으로 도달한 수준을 넘어선다고 봄.
- Anthropic 은 Sapling 을 '힘겹게 얻은 교훈(hard-won lessons)'의 출처로만 언급했고 Mononoke 는 전혀 언급하지 않음. 필자는 이를 표절에 가깝다고 평가함.
- 필자는 소개글이 Claude 가 설계를 모두 고안한 듯한 인상을 주지만, 실제로는 Meta 팀이 낸 여러 통찰을 Claude 가 거의 10년 뒤 적용한 것이라고 반박함.
- 필자는 Anthropic 이 Mononoke 설계를 복제하는 것 자체에는 반대하지 않음. 좋은 아이디어를 퍼뜨려 인류에 기여하는 것이 오픈소스 공개의 목적 중 하나이기 때문이라고 함.
- LLM 이 새 시스템 설계를 도울 때 기존 설계에 크게 의존할 가능성이 있다고 지적함. 공개된 Mononoke 는 모든 LLM 의 학습 데이터에 들어 있고, 선행 기술을 물으면 Opus 5.5 와 GPT-6 Astra 모두 Mononoke 와의 유사성을 즉시 짚는다고 함.
- 선행 작업의 공로를 인정하기는 이전보다 쉬워졌고 더 중요해졌으며, 이를 하지 않을 변명은 더 이상 없다고 결론지음.
- 비슷한 통찰을 공유하면서도 핵심 설계는 여러 면에서 다른 사례로 Vicent Marti 의 Cursor 블로그 글을 제시함.

## 자세히

필자는 2017~2018년에 팀이 구상한 설계가 오랜 시간이 지나도 유효하다는 점을 기쁘게 여긴다고 씀. 당시에는 기존 흐름을 거스르는 선택이 천재적인지 무모한지 확신하기 어려웠지만, Mononoke 설계는 시간의 검증을 견뎠다고 봄.

필자는 Mononoke 와 Sapling 을 만들 때에도 Google 의 SrcFS, 데이터베이스의 write-ahead log 등 선행 기술을 폭넓게 참고했다고 밝힘. 즉 자신들도 선행 기술을 참고했으며, 문제는 참고 자체가 아니라 출처를 밝히지 않은 것이라는 논지임.

원문에는 Anthropic 과 Scion 소개글을 직접 비교한 스크린샷이 있으나 텍스트로는 전달되지 않아, 두 설명이 실제로 얼마나 겹치는지는 필자의 주장을 통해서만 확인할 수 있음. 원문 글에는 후속 글(2026-10-15 업데이트)과 '더 많은 내용' 글이 링크되어 있으나 내용은 포함되지 않음.

## 왜 골랐나

기술혁신·특허 연구 관점에서 선행 기술(prior art) 인정과 출처 표기의 규범을 다루는 사례이며, AI 에이전트가 설계를 보조할 때 학습 데이터 속 기존 설계가 어떻게 재사용되고 어떻게 공로를 밝혀야 하는지에 대한 논점을 제공함.
