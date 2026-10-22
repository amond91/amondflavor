---
title: "Google, Gemini 4 Argon 정식 출시 - 긴 추론과 대규모 코드 작업 지원"
date: "2026-10-14"
description: "Google이 추론 모델 Gemini 4 Argon을 정식(Stable) 출시했다. 최대 약 100만 토큰의 long decode, 대규모 코드 리팩터링, 과학 연구, 3D 제작, 다단계 자율 작업을 내세우며, API 가격은 2026년 12월 1일부터 2배로 오른다."
category: "AI 에이전트·모델"
tags: ["Gemini-4-Argon", "long-decode", "코드-리팩터링", "AI-에이전트", "API-가격-인상", "벤치마크"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35306"
original: "https://deepmind.google/models/gemini/"
coverage: "summary-only"
---

> 📌 2026-10-14 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35306) · [원문 기사](https://deepmind.google/models/gemini/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Google이 추론 모델 Gemini 4 Argon을 정식(Stable) 출시했다. 최대 약 100만 토큰의 long decode, 대규모 코드 리팩터링, 과학 연구, 3D 제작, 다단계 자율 작업을 내세우며, API 가격은 2026년 12월 1일부터 2배로 오른다.

## 핵심

- API 통합 컨텍스트 한도는 1,048,576토큰이고 입력·추론·출력이 이를 공유한다. 기본 출력 한도는 65,536토큰이다. long decode는 이를 넘어 생성을 이어가는 기능이며, 한 번에 100만 토큰을 생성하는 단일 호출과는 구분된다. Interactions API에서는 백그라운드 실행을 켜면 서버가 64K 단위로 이어서 생성한다.
- Google 내부 적용 사례로 Chrome은 Chromium 약 4,000만 줄 중 600만 줄 규모 레거시 폴더에서 핵심 웹페이지 처리 코드를 분리했고, 수개월 분량의 작업을 수주에 수행했다. ADK(google/adk-python)는 세션 준비 시간 40%, 콜드 임포트 시간 21% 감소를 보고했다. Gemma 4 E2B는 A100/vLLM에서 처리량 2.85배(약 37,900 → 108,000+ 토큰/초)에 토큰 일치율 100%를 유지했다.
- 과학 분야 사례로 AlphaQubit 디코더 커널 최적화(추론 처리량 거의 2배), 스텔러레이터 관련 Grad 추측의 반례 발견(Univ. of Maryland Matt Landreman 연구팀), 희귀질환 유전체 분석(Gamow Labs), 세린 에스터레이스 설계(후속 실험에서 촉매 활성 확인)가 소개됐다. Isomorphic Labs는 PDF·특허 정보 종합 작업을 수주에서 수시간으로 단축했다고 한다.
- 공개 벤치마크는 Google 비교표 기준이다. Gemini 4 Argon은 Vals Index 68.9%, DeepSWE v1.1 77.9%, LABBench 2 88.8% 등에서 GPT-6 Astra, Claude Fable 5.1, Claude Opus 5.5보다 높다. 반면 FrontierSWE v2(65.5%는 GPT-6 Astra), Terminal-bench 4.0(66.4%는 Claude Opus 5.5), OSWorld 2.0 오프라인 부분집합(72.6%는 GPT-6 Astra) 등에서는 뒤진다. 원문도 모든 항목에서 1위는 아니라고 밝힌다.
- 표준 API 가격(100만 토큰당)은 2026년 11월 30일까지 입력 2달러, 출력·추론 10달러, 캐시 입력 0.10달러이고, 12월 1일부터 각각 4달러, 20달러, 0.20달러가 된다. 모델 ID는 gemini-4-argon이며 무료 API 등급에서는 제공되지 않고, 암묵적 캐싱만 지원한다.
- 기업 평가(Box, Deloitte, Kepler, Ambience 등)는 각 기업의 자체 평가 범위에 한정된 초기 결과이며 서로 같은 조건의 공통 벤치마크가 아니다. 학계 연구자와 독립 과학자는 Antigravity 최고 사용량 등급 6개월 무료 이용을 신청할 수 있다(선발 조건 적용).

## 자세히

Argon은 긴 추론과 대규모 코드 작업에 초점을 둔 모델이다. 입력·추론·출력이 하나의 컨텍스트 한도를 공유하므로, 긴 입력을 쓰면 추론과 출력에 남는 공간이 줄어든다. 최대 출력 설정에도 추론 토큰이 포함된다. 입력은 텍스트·이미지·영상·오디오·PDF를 지원하고 출력은 텍스트다. 이미지와 음성 생성은 별도 모델을 쓴다.

에이전트형 활용 사례로는 Antigravity의 다중 에이전트 시각적 검토 루프와 Blender MCP를 이용한 3D 제작, 책상 사진을 레이싱 게임으로 바꾸는 작업이 소개됐다. 영상에서 물리 시뮬레이션을 만들어 Apptronik Apollo 휴머노이드에 적용했을 때 7회 연속 득점했다는 사례도 있다. Cartwheel은 Gemini 4 통합으로 영상 기반 장면 생성 시간을 수시간에서 수초로 줄였다고 한다.

안전 측면에서 Google은 Frontier Safety Framework에 따라 유해 요청은 거부하면서 합법적인 이중용도 과학 연구는 유지하도록 설계했다고 밝힌다. Google 모델 중 간접 프롬프트 주입에 가장 강한 내성을 내세우며, 사고 과정과 행동을 감시해 필요하면 실행을 중단한다. 사이버 방어자 대상 Fairwind Program은 수백 곳의 추가 파트너로 확대된다.

## 왜 골랐나

AI 에이전트 개발 관점에서 ADK의 .agents/skills 규칙 기반 최적화, MCP 연동, 백그라운드 장기 실행(64K 단위 이어쓰기) 사례가 직접 관련된다. 또 Isomorphic Labs의 특허 정보 종합 사례는 특허 분석 연구와 맞닿는다. 다만 벤치마크는 Google 공개 자료다.
