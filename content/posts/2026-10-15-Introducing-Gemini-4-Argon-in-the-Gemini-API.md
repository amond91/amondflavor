---
title: "🔥 Introducing Gemini 4 Argon in the Gemini API"
date: "2026-10-15"
description: "Google이 Gemini 4 Argon(gemini-4-argon)을 Gemini API와 Google AI Studio에 전 세계 출시했습니다. 코딩·금융·법률·사이버보안·과학 분야의 장기 작업을 겨냥하며, 1M 토큰 컨텍스트 창을 갖췄고 도입 가격은 입력 100만 토큰당 $2, 출력 $10입니다."
category: "AI 에이전트·모델"
tags: ["Gemini-4-Argon", "Gemini-API", "장기-에이전트-작업", "1M-토큰-컨텍스트", "API-가격"]
source: "뉴스레터"
coverage: "full"
---

> 📌 2026-10-15 · 뉴스레터
> 근거: 메일 본문

## 한 줄 요약

Google이 Gemini 4 Argon(gemini-4-argon)을 Gemini API와 Google AI Studio에 전 세계 출시했습니다. 코딩·금융·법률·사이버보안·과학 분야의 장기 작업을 겨냥하며, 1M 토큰 컨텍스트 창을 갖췄고 도입 가격은 입력 100만 토큰당 $2, 출력 $10입니다.

## 핵심

- 유료 Gemini API 키 또는 Google AI Ultra로 사용할 수 있습니다. 모델 ID는 gemini-4-argon이며, SDK는 Python·JavaScript·Go와 cURL REST 호출을 지원합니다.
- 장기 엔지니어링·보안 작업: C++→Rust 이전이나 보안 취약점 수정 같은 목표를 주면 검증 하네스를 만들고 런타임 피드백으로 테스트하며 스스로 반복해 완료한다고 설명합니다.
- 업무 자동화: 영상·스프레드시트·PDF가 섞인 디렉터리에서 형식 간 사실을 교차 확인해 인터랙티브 대시보드나 감사(audited) 문서를 생성한다고 합니다.
- 3D·시뮬레이션·풀스택: 공간 추론과 시각적 코드 생성을 Blender, Three.js, WebGPU 등과 결합해 3D 장면, 물리 시뮬레이션, 풀스택 웹앱을 만든다고 합니다.
- 입력·사고(thinking)·출력을 1M 토큰 컨텍스트 창 안에서 통합 처리하며, 예제 코드는 `client.interactions.create`에 `generation_config={"thinking_level": "medium"}`를 지정합니다.
- 가격: 도입 가격은 입력 $2 / 출력 $10 (100만 토큰당), 캐시된 입력 토큰은 입력 가격의 95% 할인입니다. 2026년 11월 30일에 도입 기간이 끝나면 입력 $4 / 출력 $20이 적용됩니다(공개용 Argon 모델에 한함).
- 메일에는 원샷 프롬프트 예시 3개(뉴욕–런던 비행시간으로 제트기류 속도 추정, 9인 6라운드 복식 토너먼트 구성 및 Python 검증, GPS의 상대성이론 보정 설명)와 데모 앱, 문서, 개발자 가이드 링크도 있습니다.

## 자세히

메일은 Google AI Studio 팀이 발송한 제품 출시 안내이며, 성능 수치나 벤치마크는 담겨 있지 않습니다. 모델의 강점은 모두 Google의 서술로만 제시됩니다.

예제 코드는 `interactions.create` 인터페이스를 사용하며, Web Audio API로 16스텝 시퀀서·템포 슬라이더·실시간 파형 시각화가 있는 단일 HTML 드럼 머신 겸 신디사이저를 만드는 프롬프트를 보여 줍니다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 검증 하네스를 스스로 만들고 반복하는 장기 자율 작업과 다형식(영상·시트·PDF) 자료 교차 검증이라는 설명은 참고할 만합니다. 다만 실제 성능은 원문만으로 확인할 수 없습니다.
