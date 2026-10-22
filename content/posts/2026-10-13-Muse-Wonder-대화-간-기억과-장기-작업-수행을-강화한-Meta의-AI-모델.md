---
title: "Muse Wonder - 대화 간 기억과 장기 작업 수행을 강화한 Meta의 AI 모델"
date: "2026-10-13"
description: "Meta가 개인용/업무용 에이전트를 겨냥한 Muse Wonder와 Muse Wonder Full을 공개했다. 대화 간 기억(Active Memory)과 장기 작업 수행(Active Focus)을 앞세우고, 코딩·법률·금융·건강 분야에서 자체 벤치마크 최고 수준을 주장한다."
category: "AI 에이전트·모델"
tags: ["Muse-Wonder", "Active-Memory", "Active-Focus", "AI-에이전트", "SWE-Marathon", "Meta"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35251"
original: "https://research.meta.ai/blog/muse-wonder"
coverage: "full"
---

> 📌 2026-10-13 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35251) · [원문 기사](https://research.meta.ai/blog/muse-wonder)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Meta가 개인용/업무용 에이전트를 겨냥한 Muse Wonder와 Muse Wonder Full을 공개했다. 대화 간 기억(Active Memory)과 장기 작업 수행(Active Focus)을 앞세우고, 코딩·법률·금융·건강 분야에서 자체 벤치마크 최고 수준을 주장한다.

## 핵심

- Muse Wonder는 Meta Model API와 Muse Code에서, Muse Wonder Full은 Muse Code 구독자에게 제공된다. Muse 서비스에는 구독자부터 순차 적용하고, 결과물(artifact) 생성과 더 높은 지능이 필요한 처리에도 쓸 계획이다.
- Active Memory는 중요한 사용자 맥락을 대화 간에 유지하고, 과거 정보가 현재 작업과 관련될 때 능동적으로 검색한다. Active Focus는 대안 시도, 장애물 해결, 새 정보 반영을 이어가며, 필요하면 추가 설명을 요청하고 중요한 행동 전에 사용자에게 확인한다.
- Meta 자체 평가(GeekNews 요약 기준)에서 DeepSWE 1.1은 Full 79.0점 / Wonder 78.8점, 장기 소프트웨어 작업을 시험하는 SWE-Marathon은 Full 57.5점 / Wonder 53.1점이다. 집중력은 FocusBench, 법률 업무는 Harvey의 Legal Agent Benchmark(LAB)로 평가했다.
- 일반화 실험에서는 추론 시 연산량을 늘리면 두 모델 모두 사후학습 때 겨냥하지 않은 작업(out-of-distribution)의 성능이 비슷하게 오른다. 다만 Muse Wonder Full은 학습 분포 내 성능 대비 Muse Spark 1.3의 추세가 예측하는 것보다 낯선 작업에서 더 높다. Meta는 이를 더 크고 강한 기반 모델 덕분이라고 해석한다.
- 두 모델은 기가와트 규모의 Prometheus 데이터센터에서 학습했다. Meta는 14개월 전 AI 개발 체계를 전면 개편했다고 밝혔다.
- 안전 측면에서는 적대적 지시에 대한 견고성과 개인정보의 신중한 처리를 목표로 학습했다. Meta는 자사 최강의 CBRN·사이버 오용 방지 장치를 적용했고, 방어 목적의 최첨단 사이버 역량 접근은 유지한다고 밝혔다. 또 가장 정렬이 잘된 모델이라 주장하며 전체 안전성 보고서는 곧 공개할 예정이다.
- 시연은 이미지 입력 기반 3D 주방 게임 제작, 브라우저·여러 앱을 오가며 짠 맞춤형 달리기 계획, Instagram 프로필 사진으로 기념품 디자인·주문, 복잡한 조건의 결혼식 좌석 배치도 작성이다.

## 자세히

Meta는 'benchmaxxing'(벤치마크 과최적화) 비판과 'big model smell'(대형 모델 특유의 폭넓은 역량) 호평이 모두, 인기 벤치마크가 포착하지 못하는 범용성 차이를 가리킨다고 설명한다. 이를 정량화하려고 추론 노력 수준별로 학습 분포 내 종합 점수(가로축)와 분포 밖 작업 평균 점수(세로축)를 비교했다. 개인용 에이전트는 학습 중 다 예상할 수 없는 목표·도구·제약의 조합을 다뤄야 하므로 이 범용성이 중요하다는 논리다.

코딩은 요구사항 추적, 코드베이스 전반의 변경, 반복적인 테스트와 수정을 요구하며, 메모리와 지속적 작업 수행 능력을 함께 시험하는 영역으로 소개된다. 두 모델은 다양하고 장시간에 걸친 과제로 학습했다고 한다.

주의할 점은 벤치마크 수치가 모두 Meta 자체 평가라는 것이다. 원문 본문에는 비교표와 그래프의 구체 수치가 없고, 세부 내용은 별도 모델 보고서에 있다고만 되어 있다. 위 점수는 GeekNews 요약 페이지에서 가져왔다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 대화 간 장기 기억과 장시간 작업 지속(SWE-Marathon, FocusBench)을 평가 축으로 내세운 점과 중요한 행동 전 사용자 확인을 설계에 넣은 점이 참고가 된다. 기술경영 관점에서는 Prometheus 데이터센터 투자와 14개월간의 AI 조직·스택 재구축이 대형 기술기업의 전략 사례로 읽힌다.
