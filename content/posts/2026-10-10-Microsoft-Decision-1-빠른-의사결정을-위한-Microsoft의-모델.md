---
title: "Microsoft-Decision-1, 빠른 의사결정을 위한 Microsoft의 모델"
date: "2026-10-10"
description: "Microsoft가 라우팅·분류·우선순위 지정·검증·워크플로 제어용 결정 채점 모델 Microsoft-Decision-1을 Foundry와 OpenRouter에 출시했다. Qwen3.5-9B를 추가 학습해 선택지마다 보정된 확률을 한 번의 처리로 반환하며, Microsoft 자체 평가에서 36개 벤치마크 평균 정확도 83.5%, 중앙값 지연 85ms로 비교 모델 중 가장 빨랐다."
category: "AI 에이전트·모델"
tags: ["Microsoft-Decision-1", "decision-model", "모델-라우팅", "에이전트-제어", "확률-보정", "Microsoft-Foundry"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35100"
original: "https://commandline.microsoft.com/microsoft-decision-1-model-foundry/"
coverage: "partial"
---

> 📌 2026-10-10 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35100) · [원문 기사](https://commandline.microsoft.com/microsoft-decision-1-model-foundry/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Microsoft가 라우팅·분류·우선순위 지정·검증·워크플로 제어용 결정 채점 모델 Microsoft-Decision-1을 Foundry와 OpenRouter에 출시했다. Qwen3.5-9B를 추가 학습해 선택지마다 보정된 확률을 한 번의 처리로 반환하며, Microsoft 자체 평가에서 36개 벤치마크 평균 정확도 83.5%, 중앙값 지연 85ms로 비교 모델 중 가장 빨랐다.

## 핵심

- 정해진 선택지와 현재 상태를 입력하면 각 선택지에 보정된 확률 점수를 반환한다. 예/아니요, 객관식, 평점과 평가 기준에 따른 AI 응답·에이전트 행동 채점을 지원한다. 확률에 따라 자동 실행, 결정 보류, 사람 검토 요청 같은 흐름을 만들 수 있다.
- Qwen3.5-9B를 사후 학습해 단일 패스 채점에 최적화했다. 향후 Microsoft AI(MAI)와 OpenAI를 포함한 다른 기반 모델로 전환할 계획이다.
- 학습에 노출하지 않은 36개 벤치마크, 147,137개 문항의 평균 정확도는 83.5%로 1위였다. 2위는 Jev 1.13.0(82.3%), 3위는 Quyet-1.0-Large(81.9%)다. 정확도는 모두 Microsoft 자체 평가 결과다.
- 중앙값 지연 시간은 85ms(P95 125ms)였다. 지연 시간 비교 기준은 JevBench v1.6.1의 보정된 중앙값이고, Microsoft-Decision-1은 Foundry 동일 리전에서 별도로 측정했다. 기사는 H2O-Lightning-4B v1.1보다 약 2.5배, GPT-6 Sol(3.01초)보다 약 35배 빠르다고 밝힌다. 순차 워크플로에서는 결정마다 지연이 누적되므로(20개 결정에 각 100ms면 2초) 이 속도가 중요하다고 설명한다.
- 같은 의미의 입력을 8가지 방식으로 변형했을 때 평균 판정 변경률은 1.3%였다. 확률 보정 점수는 Jev 93.7, Quyet 93.1, Microsoft-Decision-1 92.2로 3위였다. 안전성은 11개 벤치마크, 5,250개 요청으로 평가했고, 자체 평가에서 유해 행동은 거부하면서 무해한 요청의 유용성은 유지했다고 한다.
- Microsoft 내부 적용 사례: Xbox Research의 피드백 분류는 GPT-6 Sol과 경쟁력 있는 품질에 14배 이상 빠르고 비용은 200분의 1이었다. Copilot 팀의 응답 품질 평가는 GPT5.6 Luna와 경쟁력 있는 품질에 100배 빨랐다. Microsoft Discovery의 적응형 재계획은 LLM 기반 채점보다 일관성이 46배 높고 채점이 3배 빨랐으며, 전체 속도는 약 4배 향상됐다.
- 가격은 입력 토큰 100만 개당 0.042달러이고 출력 토큰은 무료다. 품질·확신도·비용을 개선하는 모델 업데이트를 이어갈 계획이다.

## 자세히

원문 기사는 결정 모델(decision model)을 텍스트 생성이나 복잡한 추론을 위한 LLM과 구분되는 새 범주로 소개한다. 소프트웨어가 바로 실행할 수 있는 구조화된 출력을 아주 낮은 비용과 높은 성능으로 내는 것이 목적이라고 한다. 필자는 Microsoft CTO실 소프트웨어 엔지니어링 부사장 Achint Srivastava이고, 게시일은 2026.10.09다. 이후 Jev의 정확도와 보정 벤치마크를 추가해 수정했다.

활용처로 에이전트 제어와 모델 라우팅(실행·중단·재시도·인계 판단, 품질·비용·지연에 맞는 모델 선택), 데이터 분류와 품질 평가, 검색 관련도 채점과 추천, 장애 유형·긴급도 분류와 위험도 표시, 컴퓨터 사용·로보틱스·과학 연구(후보 가설·화합물·실험의 후속 평가 순위 결정)를 든다.

비교표의 다른 모델은 Surogate Rune 26B-A4B(79.7%, 380ms), GPT-6 Luna Decisions(79.4%, 300ms), deck-31B(77.8%, 400ms), H2O-Lightning-4B v1.1(77.2%, 210ms), Strands-Decider 2B(54.8%, 지연 미측정)다. Strands-Decider 2B의 정확도는 답할 수 있었던 23개 벤치마크만의 평균이라 직접 비교하기 어렵다. 정확도와 안전성 수치는 모두 Microsoft 자체 평가이고 제3자 검증은 언급되지 않는다. 확률 보정에서는 1위가 아니라는 점도 함께 봐야 한다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 에이전트의 실행·중단·재시도·인계와 모델 라우팅을 보정된 확률로 제어하는 경량 결정 레이어 사례다. 확률 임계값으로 자동 실행과 사람 검토를 가르는 설계가 에이전트 워크플로 설계와 직접 맞닿는다. 또한 '결정 모델'이 LLM과 구분되는 새 범주로 부상한다는 주장은 기술혁신·기술경영 연구에서 볼 만한 동향이다.
