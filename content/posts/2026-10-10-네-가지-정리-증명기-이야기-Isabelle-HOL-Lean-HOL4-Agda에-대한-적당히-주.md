---
title: "네 가지 정리 증명기 이야기: Isabelle/HOL, Lean, HOL4, Agda에 대한 (적당히) 주관적인 비교"
date: "2026-10-10"
description: "유클리드의 소수 무한성 증명을 Isabelle/HOL, Lean, HOL4, Agda 네 정리 증명기로 형식화해 사용자 경험을 비교한 주관적 평가다. 필자는 자동화와 정리 검색이 강한 HOL4를 개인적 종합 1순위로 꼽았고, 가능하면 네 도구 모두, 적어도 기존과 다른 방식 하나는 써 보라고 권한다."
category: "연구·과학"
tags: ["정리-증명기", "Isabelle/HOL", "Lean", "HOL4", "Agda", "형식-검증"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35118"
original: "https://blueberrywren.dev/blog/primes/"
coverage: "partial"
---

> 📌 2026-10-10 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35118) · [원문 기사](https://blueberrywren.dev/blog/primes/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

유클리드의 소수 무한성 증명을 Isabelle/HOL, Lean, HOL4, Agda 네 정리 증명기로 형식화해 사용자 경험을 비교한 주관적 평가다. 필자는 자동화와 정리 검색이 강한 HOL4를 개인적 종합 1순위로 꼽았고, 가능하면 네 도구 모두, 적어도 기존과 다른 방식 하나는 써 보라고 권한다.

## 핵심

- 네 도구는 두 계열로 나뉜다. Lean과 Agda는 의존 타입과 Curry-Howard 대응을 쓰고, Isabelle/HOL과 HOL4는 작은 증명 커널의 기본 규칙으로 증명을 만드는 LCF 방식이다. 필자는 비교 전에 Isabelle/HOL과 Agda에 가장 익숙했고, HOL4와 Lean은 이번에 일부 배웠다고 밝힌다.
- 자동화 수준은 Isabelle/HOL과 HOL4가 높고 Lean은 중간, Agda는 거의 없음이다. sledgehammer와 Lean의 grind는 목표를 못 풀면 진행 결과를 남기지 않는다. 필자는 simp, auto, gvs처럼 일부만 처리하고 정리된 문맥을 남기는 부분 자동화를 더 중요하게 봤고, 이 점에서 Lean이 아쉬웠다고 한다.
- Isabelle/HOL(find_theorems)과 HOL4(DB.find, DB.match)는 `_ < SUC _` 같은 명제 형태 패턴으로 정리를 검색할 수 있어 편리했다. Lean의 leansearch와 loogle은 내장 기능이 아니고 만족도가 낮았으며, Agda는 대응 기능이 없다.
- Isabelle/HOL과 HOL4는 배중률과 Hilbert의 엡실론을 공리로 채택한 고전 논리 기반이라 자동 솔버에 유리하다고 느꼈다. Agda는 구성적으로 유지해 증명에 수를 넣으면 더 큰 소수를 실제로 계산해 준다. 다만 입력 8에 결과 19를 얻는 데 40초가 걸렸고(입력 6→71은 3초, 7→61은 10초), 소수 판정 절차도 직접 짜야 했다.
- 구현 규모는 Isabelle/HOL 117줄(증명 19개), Lean 155줄(12개), HOL4 190줄(16개), Agda 264줄(45개, where 블록 제외 시 29개)이다. 동일 조건의 비교는 아니다.
- 다섯 가지 주관적 순위는 다음과 같다. 증명 발견 용이성은 Isabelle/HOL, HOL4, Lean, Agda 순이다. 증명 조작 용이성은 Agda, Lean, HOL4, Isabelle/HOL 순이고, 즐거움은 HOL4, Lean, Isabelle/HOL, Agda 순이다. 짜증은 Agda가 가장 높고 Isabelle/HOL이 가장 낮았다. 예상 밖 동작으로 인한 당황은 HOL4가 가장 컸고 Agda가 가장 예측 가능했다.
- Lean에서 두 수의 차가 나눠진다는 단순한 보조정리(divides_sub)를 증명하는 데 가장 많은 노력이 들었다. 필자는 보조정리 검색의 어려움과 문법 학습 중이었던 점이 영향을 줬다고 덧붙인다.

## 자세히

증명 구조는 네 도구 모두 비슷하다. 나눗셈 관계, 리스트 곱, 소수를 정의하고, 리스트의 원소가 곱을 나눈다는 보조정리와 1보다 큰 비소수에 소인수가 존재한다는 보조정리를 증명한다. 마지막으로 리스트 원소의 곱에 1을 더한 수를 이용해 리스트 밖의 소수를 찾는다. 그래서 필자는 증명 내용보다 사용자 경험이 비교의 주된 차이라고 본다. 이 마지막 단계에서 Isabelle/HOL 증명이 가장 짧았지만 가장 읽기 어려웠다고 한다.

도구별 증명 스타일도 대비된다. Agda는 명시적인 증명 항을 직접 쓰므로 동작이 예측 가능하고, Emacs의 C-c C-c 경우 분리 기능이 유용했다. Agda 합성수 정의에는 더 작은 소인수와 그 증거를 넣어 뒤에 바로 꺼내 쓸 수 있게 했다. HOL4 증명은 SML 항이며, `>>`(앞 전술이 만든 모든 하위 목표에 적용)와 `>-`(하위 목표 하나에 적용) 같은 함수로 전술을 조합한다. REPL에서 동적으로 구성한 뒤 함수 조합을 맞춰 완성해야 한다.

필자는 의존 타입 기반 도구만 써 봤다면 Isabelle/HOL이나 HOL4를, LCF 방식만 써 봤다면 반대편을 시도해 보라고 권한다. Lean처럼 완전히 대화형인 도구만 써 봤다면 HOL4나 Agda가 다른 경험을 줄 것이라고 한다. 필자는 가장 능숙한 도구가 업무에서도 쓰는 Isabelle/HOL이라고 밝힌다. 이 글은 개인 평가이므로 공정한 성능 비교로 보기 어렵고, 필자도 증명이 모범 답안이 아니라고 인정한다.
