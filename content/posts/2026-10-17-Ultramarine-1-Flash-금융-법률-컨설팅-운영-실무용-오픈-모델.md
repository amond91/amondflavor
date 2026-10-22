---
title: "Ultramarine 1 Flash - 금융, 법률, 컨설팅, 운영 실무용 오픈 모델"
date: "2026-10-17"
description: "Paper Instruments가 GLM-5.3 Flash에 금융·법률·컨설팅·운영 업무용 대규모 강화학습을 적용해 만든 오픈 모델 Ultramarine 1 Flash를 공개했다. 자체 벤치마크 PI Knowledge Work에서 작업당 0.13달러로 59.3점을 기록해 Claude Fable 5(57.8점, 8.19달러)보다 높았다고 주장하지만, 모델 가중치는 아직 공개되지 않았다."
category: "AI 에이전트·모델"
tags: ["Ultramarine-1-Flash", "Paper-Instruments", "GLM-5.3-Flash", "강화학습", "지식-업무-에이전트", "PI-Knowledge-Work"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35519"
original: "https://www.paperinstruments.com/ultramarine"
coverage: "partial"
---

> 📌 2026-10-17 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35519) · [원문 기사](https://www.paperinstruments.com/ultramarine)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Paper Instruments가 GLM-5.3 Flash에 금융·법률·컨설팅·운영 업무용 대규모 강화학습을 적용해 만든 오픈 모델 Ultramarine 1 Flash를 공개했다. 자체 벤치마크 PI Knowledge Work에서 작업당 0.13달러로 59.3점을 기록해 Claude Fable 5(57.8점, 8.19달러)보다 높았다고 주장하지만, 모델 가중치는 아직 공개되지 않았다.

## 핵심

- Ultramarine 1 Flash는 GLM-5.3 Flash 기반으로, 답변뿐 아니라 Excel·PowerPoint·Word 결과물과 ERP·MES 같은 업무 시스템 내 작업에 초점을 맞춘 에이전트 모델이다.
- 학습은 19개 실무 분야, 88개 환경 유형으로 구성했다. 코드처럼 단위 테스트로 정답을 가리기 어려워 과제마다 내용·정확성·요구사항 충족을 판단하는 평가 기준(rubric)을 함께 생성했다. 학습·평가에는 자체 실행 환경 Feather를 썼다.
- PI Knowledge Work(자체 평가) 점수와 작업당 비용은 다음과 같다. Ultramarine 59.3점·0.13달러, Claude Fable 5 57.8점·8.19달러, GPT-5.6 Sol 55.1점·2.09달러, Claude Opus 5.5 61.9점·6.96달러, GPT-6 Sol 61.3점·1.26달러. 비교 모델은 모두 최대 추론 설정이며, Fable 5 대비 비용은 약 60분의 1이다. Opus 5.5·GPT-6 Sol보다는 각각 2.6점, 2.0점 낮다.
- 6개 지식 업무 벤치마크(JobBench, PI Knowledge Work, FrontierFinance, AutomationBench, DiligenceBench, APEX-Agents 1.1) 모두에서 기반 모델보다 개선됐고 평균 4.1점 올랐다. 추론 포함 출력 토큰은 32.7M에서 30.5M으로 6.5% 줄었다.
- 모든 벤치마크에서 앞선 것은 아니다. AutomationBench는 DeepSeek V4.1 Flash(51.3)에 이은 2위(50.2)이고, DiligenceBench는 3위(50.6)다. APEX-Agents 1.1은 Gemini 3.8 Flash(64.3)와 Claude Fable 5(63.6)에 이은 3위(57.0)다.
- API 가격은 100만 토큰당 입력 0.15달러, 캐시 입력 0.03달러, 출력 0.50달러로 GLM-5.3 Flash와 같다. GitHub 저장소와 기술 보고서는 있으나, 확인 시점(2026년 10월 18일) 기준 Hugging Face 버튼은 비활성이고 "Model weights coming soon"으로 표시된다.

## 자세히

PI Knowledge Work는 Paper Instruments가 직접 만든 내부 벤치마크다. 과제별로 작성한 rubric이 내용·정확성·표현을 채점하고, 모든 모델이 동일한 Feather 환경에서 돌아간다. 비용은 각 개발사의 공식 API 가격으로 계산했다. 이 벤치마크 점수는 Artificial Analysis Intelligence Index와 상관계수 r = 0.81로 상관된다고 한다. 이는 24개 실행을 비교한 값이다.

비용·성능 면에서는 모델과 추론 설정을 조합한 다른 39개 실행 중 27개가 Ultramarine보다 비용이 높고 점수가 낮았다고 한다. 공식 페이지는 기업 실적 분석 슬라이드, 항공사 운영 모델, 투자 검토 메모, DCF 기반 기업가치 평가 등의 결과물 예시를 싣고 있다. 그중 Nucor 사례는 5개 기간의 부문별 매출·이익을 한 표에 담았고, 모든 소계와 합계가 맞아떨어지게 만들었다고 한다.

기반 모델의 공개 점수를 쓴 일부 벤치마크에서는 해당 점수에 가장 가까운 자체 실행의 토큰 수를 비교에 썼다. 학습 데이터 구성과 평가 방식은 기술 보고서에 있다고 하나, 이번에 받은 원문에는 보고서 본문이 포함되지 않았다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 단위 테스트로 검증할 수 없는 업무를 과제별 rubric으로 평가해 강화학습에 쓰는 방식과 Feather 같은 에이전트 실행 환경 설계가 참고할 만하다. 다만 핵심 성능 수치가 개발사 자체 벤치마크에 의존한다.
