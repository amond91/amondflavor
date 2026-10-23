---
title: "OpenAI 모델, Google Cloud에 도입 예정"
date: "2026-10-22"
description: "OpenAI와 Google Cloud가 협력해 OpenAI의 최신 모델(GPT-6 Astra, GPT-6.1 Sol, GPT-6 Luna)과 Codex 에이전트 워크플로를 Google Cloud Model Garden에 도입한다. 일부 기업 고객 대상 비공개 프리뷰가 시작되었고 정식 출시는 향후 몇 주 내로 예정이다."
category: "AI 에이전트·모델"
tags: ["OpenAI", "Google-Cloud", "Model-Garden", "Codex", "GPT-6", "에이전트-워크플로"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35775"
original: "https://openai.com/index/openai-models-google-cloud/"
coverage: "summary-only"
---

> 📌 2026-10-22 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35775) · [원문 기사](https://openai.com/index/openai-models-google-cloud/)
> 근거: GeekNews 요약 페이지 — ⚠️ 원문 기사: HTTP Error 403: Forbidden

## 한 줄 요약

OpenAI와 Google Cloud가 협력해 OpenAI의 최신 모델(GPT-6 Astra, GPT-6.1 Sol, GPT-6 Luna)과 Codex 에이전트 워크플로를 Google Cloud Model Garden에 도입한다. 일부 기업 고객 대상 비공개 프리뷰가 시작되었고 정식 출시는 향후 몇 주 내로 예정이다.

## 핵심

- Google Cloud의 Model Garden(여러 공급업체의 AI 모델을 찾아 선택·활용하는 모델 카탈로그)에서 OpenAI 모델과 Codex를 쓸 수 있게 된다. 개발자와 데이터 과학자는 기존 Google Cloud 환경에서 소프트웨어 개발 자동화와 다단계 에이전트 구축이 가능하다.
- 비공개 프리뷰 모델은 세 가지다. GPT-6 Astra는 코딩·컴퓨터 조작·전문 업무·과학 연구 등 난도 높은 작업용이고, GPT-6.1 Sol은 Astra에 가까운 지능을 더 낮은 비용으로 제공하며, GPT-6 Luna는 일상 작업을 대규모로 처리하는 빠르고 비용 효율적인 모델이다.
- 구매는 Model Garden에서 모델을 선택하고 Google Cloud Marketplace로 한다. 기존 Google Cloud 지출 약정과 Flexible Savings Plans을 적용할 수 있고, 이용 내역은 하나의 Google Cloud 청구서로 통합된다.
- 로컬 Codex CLI와 데스크톱 앱을 Google Cloud에 연결해 추론을 실행하거나, API로 자체 애플리케이션과 에이전트를 만들 수 있다.
- OpenAI 모델은 전용 Google Cloud 인프라에서 실행된다. 프롬프트·맥락·출력은 지정된 데이터 경계 안에 유지되며 기반 모델 학습에 쓰이지 않고, 인증된 ID·VPC Service Controls·통합 감사 로그로 접근을 관리한다. 데이터 무보관(ZDR) 옵션과 Confidential Compute는 추후 제공 예정이다.
- Google Antigravity와 기업용 Gemini(Gemini Agent)에서도 OpenAI 모델을 제공할 계획이며, 현재 접근 신청을 받고 있다. Microsoft는 계속 OpenAI의 주요 클라우드 파트너로 유지된다.

## 자세히

이번 발표는 OpenAI 발표와 Google Cloud의 10월 22일 발표를 함께 정리한 것이다. Google Cloud 쪽 발표에서 제공 모델과 실행 환경이 구체화되었으며, 프리뷰는 일부 기업 고객에게 순차적으로 제공된다.

주요 대상에는 민감한 데이터의 접근 주체와 사용 방식을 통제해야 하는 규제 산업 기업이 포함된다. OpenAI 모델에 Google Cloud의 접근 제어, 중앙화된 데이터 관리, 고성능 인프라를 결합하는 것이 핵심 가치로 제시된다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 하나의 클라우드 플랫폼에서 여러 공급업체의 모델과 Codex 에이전트 워크플로를 고를 수 있게 되는 점이 관련된다. 기술경영·기술정책 관점에서는 모델 공급자와 클라우드 사업자 간 제휴 구조 변화를 보여주는 사례로 볼 수 있다.
