---
title: "OpenAI와 Anthropic을 단호히 거부하자"
date: "2026-10-13"
description: "저자(Micah Lee)는 OpenAI·Anthropic 대신 GLM-5.3 등 오픈 웨이트 모델을 쓰자고 주장한다. 이들 모델은 폐쇄형 모델보다 약 4개월 뒤처질 뿐이고, 프라이버시(Tinfoil 등 보안 인클레이브 추론)와 가드레일 측면에서 낫다는 것이다. 근거는 벤치마크가 아니라 짧은 기간의 개인 사용 경험이다."
category: "AI 에이전트·모델"
tags: ["오픈-웨이트-모델", "GLM-5.3", "Tinfoil", "보안-인클레이브", "사이버보안-가드레일", "프라이버시"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35275"
original: "https://micahflee.com/just-say-no-to-openai-and-anthropic/"
coverage: "full"
---

> 📌 2026-10-13 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35275) · [원문 기사](https://micahflee.com/just-say-no-to-openai-and-anthropic/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

저자(Micah Lee)는 OpenAI·Anthropic 대신 GLM-5.3 등 오픈 웨이트 모델을 쓰자고 주장한다. 이들 모델은 폐쇄형 모델보다 약 4개월 뒤처질 뿐이고, 프라이버시(Tinfoil 등 보안 인클레이브 추론)와 가드레일 측면에서 낫다는 것이다. 근거는 벤치마크가 아니라 짧은 기간의 개인 사용 경험이다.

## 핵심

- 오픈 웨이트 모델은 OpenAI·Anthropic의 최신 모델보다 약 4개월 뒤처지는 것으로 보이며, 저자는 이 격차를 크지 않다고 본다. 취약점 연구 같은 복잡한 작업에서도 근접한 성능이라고 평가한다.
- 저자의 사용 감각(벤치마크 아님)으로 GLM-5.3은 소프트웨어 엔지니어링에서 Claude Opus 5.5와 대체로 비슷하고, GLM-5.3-Flash는 Claude Opus 4.8 수준이다. 512GB RAM Mac Studio가 있으면 개인이 직접 실행할 수도 있다.
- 학습 사용 거부(opt-out)는 비공개 대화를 뜻하지 않는다. 두 회사는 대화 전체에 접근할 수 있고 정부·기업과 공유할 수도 있으며, 학습 거부를 장기적으로 신뢰할 수 있는지도 별개의 문제라고 지적한다.
- 사이버보안 용도에는 대화를 감시하는 에이전트가 세션을 막는 가드레일이 적용되고 오탐도 잦다. OpenAI의 Trusted Access for Cyber와 Anthropic의 Cyber Verification Program으로 접근 승인을 받은 쪽만 제한이 느슨한 모델을 쓰는데, 저자는 이 결정권을 Sam Altman과 Dario Amodei에게 맡기는 것이 비민주적이라고 비판한다.
- 프라이버시가 중요하면 보안 인클레이브 기반 추론을 쓴다. 코딩 에이전트 작업에는 토큰 과금의 Tinfoil(GLM-5.3은 100만 토큰당 입력 $1.80, 캐시 $0.80, 출력 $5.75)을, 챗봇형 사용에는 Tinfoil 추론을 내부적으로 쓰는 Maple AI(무료 플랜, 월 $20 Pro)를 권한다.
- 프라이버시가 덜 중요하면 OpenCode Go(월 $10, 추론 라우터)를 쓸 수 있다. 라우터와 백엔드 제공업체 모두 세션 데이터에 접근하며, 0일 보존 등은 정책상 약속일 뿐 기술적 보장은 아니다.
- 개발사 신뢰는 모델 자체의 안전성과 구분해야 한다. Z.ai는 ZCode가 동의 없이 로컬 소스 저장소를 Alibaba Cloud에 업로드한 사실이 드러나 직접 계정을 만들지 말라고 하고, DeepSeek·Qwen·Kimi도 모델은 안전하나 개발사에 대화를 맡기는 것은 다르다고 한다. 유일한 비미국·비중국 주요 업체인 Mistral(Mistral Large 4, 별칭 Le Chonk)은 상대적으로 신뢰할 만하다고 본다.
- 저자의 실제 운용은 OpenCode 하네스에 GLM-5.3(low 추론)을 쓰는 방식이다. 이미 오픈소스인 공개 작업에는 OpenCode Go를, 그 외 모든 작업에는 Tinfoil을 쓴다.

## 자세히

글의 출발점은 기술 정책적·정치적 입장이다. 저자는 Naomi Klein과 Astra Taylor의 『End Times Fascism and the Fight for the Living World』를 읽고, 미국 프런티어 AI 연구소 지도부가 'Machine God'과 AGI에 집착하며 자신들만 이를 안전하게 통제할 수 있다고 믿는다고 강하게 불신하게 되었다고 밝힌다. 이 글의 불매 권고는 성능 비교가 아니라 이 불신에서 나온다. 기술적 주장은 그 뒤에 따라온다.

근거의 한계도 분명하다. 성능 비교는 '느낌 기반(vibes-based)' 평가이고, 이 방식으로 일한 기간도 '아직 짧다'고 저자 스스로 말한다. 보안 인클레이브는 완벽하지 않지만, 제공업체가 모든 데이터에 제한 없이 접근하는 기존 방식보다는 의미 있는 차이가 있다는 것이 저자의 입장이다. 모델 변화가 빨라 6개월 뒤의 최선은 알 수 없다고 덧붙인다.

저자의 잠정 결론은, 개인적 사용 범위에서 오픈 웨이트 모델이 ChatGPT와 Claude를 완전히 대체할 수 있어 보이며 비용은 비슷하거나 더 낮고 프라이버시는 높고 가드레일은 훨씬 적다는 것이다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 코딩 하네스(OpenCode)에 제공업체를 바꿔 끼우는 구성과 작업 공개 여부에 따라 추론 제공업체를 나누는 방식이 참고가 된다. 기술정책 관점에서는 사이버보안 모델 접근 승인을 민간 기업이 결정하는 구조에 대한 비판이 논점이 된다.
