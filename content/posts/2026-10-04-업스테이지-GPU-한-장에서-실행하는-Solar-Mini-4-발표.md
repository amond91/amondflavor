---
title: "업스테이지, GPU 한 장에서 실행하는 Solar Mini 4 발표"
date: "2026-10-04"
description: "업스테이지가 총 35B 파라미터 중 토큰당 3B만 활성화하는 MoE 에이전트 모델 Solar Mini 4를 발표했다. 반복적인 정보 조회·분류·도구 호출 작업을 낮은 비용으로 처리하도록 설계했고, 양자화 모델은 H100 80GB 한 장에서 배포할 수 있다."
category: "AI 에이전트·모델"
tags: ["Solar-Mini-4", "Upstage", "MoE", "AI-에이전트", "도구-호출", "온프레미스-배포"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=34738"
original: "https://www.upstage.ai/blog/ko/solar-mini-4"
coverage: "full"
---

> 📌 2026-10-04 · GeekNews · [원문 보기](https://news.hada.io/topic?id=34738) · [원문 기사](https://www.upstage.ai/blog/ko/solar-mini-4)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

업스테이지가 총 35B 파라미터 중 토큰당 3B만 활성화하는 MoE 에이전트 모델 Solar Mini 4를 발표했다. 반복적인 정보 조회·분류·도구 호출 작업을 낮은 비용으로 처리하도록 설계했고, 양자화 모델은 H100 80GB 한 장에서 배포할 수 있다.

## 핵심

- Solar Mini 4는 업스테이지가 독자적으로 사전학습한 모델이다. 35B 중 토큰당 3B만 활성화하는 MoE 구조이고, 512K 컨텍스트와 128K 출력을 지원한다.
- Artificial Analysis Intelligence Index v4.3.2에서 24.1점으로, 점수가 확인된 활성 3B 모델 중 가장 높다. Gemma 4 31B보다 9.1점, Nemotron 3 Ultra(활성 55B)보다 1.1점 높고, MiMo-V2.5보다는 0.9점 낮다.
- 에이전트 평가로는 AutomationBench-AA 22.3%, τ³-Banking 47.2점(Index와는 별도 평가)을 기록했다. 세부 평가는 AA-LCR 83.3, SciCode 47.6, Humanity's Last Exam 25.8이다.
- JSON 모드와 JSON Schema 구조화 출력, 도구 호출, 병렬 도구 호출(parallel_tool_calls=True)을 지원한다. 추론은 기본적으로 꺼져 있고 reasoning_effort로 조절한다.
- 배포 조건에서 주의할 점이 있다. 연산에는 3B만 쓰지만 메모리에는 전체 35B 가중치를 올려야 한다. 최소 구성은 양자화 모델 기준 H100 80GB 한 장이다. 내부 처리량 측정(H100 두 장, 동시 요청 32건, 요청당 70 tokens/s 이상)은 4K 입력·1K 출력, prefix caching 없이 한 것으로 최소 사양과는 별개의 구성이다.
- API 정가는 100만 토큰당 입력 $0.10, 캐시 입력 $0.01, 출력 $0.40이다. Upstage Console API, Solar Chat, OpenRouter와 온프레미스로 제공한다. Console은 UTC 기준 10월 10일까지 70% 할인하고 10월 11일부터 정가로 전환한다.

## 자세히

업스테이지는 토큰 가격만이 아니라 작업 하나를 끝내는 데 쓰는 호출 횟수와 토큰 수가 에이전트 운영비를 좌우한다고 설명한다. 이를 보이려고 MiMo-V2.5와 내부 비교를 했다. 한국어 태스크 3건(코드 분석·리뷰, 다중 도구 일정 조율 등)을 모델별로 3회씩 실행했고, 두 모델 모두 품질 체크포인트를 통과했다. 정가 기준 1,000세트 완료 비용은 Solar Mini 4가 $1.25, MiMo-V2.5가 $2.20으로 약 43% 낮았다.

다만 이 비교는 저자도 '초기 내부 테스트'라고 밝힌다. temperature=0이고 프롬프트 캐싱은 적용하지 않았다. MiMo V2.5는 외부 라우터를 거쳤기 때문에 지연시간은 비교에서 제외했다. 실제 환경과 작업 난도에 따라 결과가 달라질 수 있다고 명시한다.

권장 용도는 계약서 항목 추출, 고객 문의 분류, 결재 상태 조회와 규정 적용, Document Parse·Information Extract 결과를 검증하는 문서 파이프라인 단계다. 업스테이지는 의사결정에 영향을 주는 업무에서는 출력을 최종 판단으로 쓰지 말고 별도 검증 절차를 두라고 권고한다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 활성 파라미터 대비 성능과 작업 완료 비용(토큰·턴 수)이라는 평가 틀이 참고가 된다. 다만 비용 비교와 처리량은 모두 업스테이지의 내부 측정이다.
