---
title: "Antigravity의 /teamwork로 과학과 엔지니어링 발전시키기"
date: "2026-11-15"
description: "Google과 학계 연구진이 Gemini 4(Argon·Carbon·Hydrogen)와 Antigravity의 /teamwork 멀티에이전트 시스템으로 수학 난제 증명, 분자 구조 규명, Rust 재작성, 인프라 최적화에서 성과를 냈다고 밝혔다. 병렬 탐색과 반증 수단(Lean 4, 원본 테스트 등), 에이전트별 문맥 분리가 핵심 설계다."
category: "연구·과학"
tags: ["Antigravity-/teamwork", "Gemini-4", "멀티에이전트", "Lean-4-형식-검증", "Rust-재작성", "Deep-Review"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=37191&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://research.google/blog/advancing-science-engineering-with-antigravitys-teamwork-feature/"
coverage: "partial"
---

> 📌 2026-11-15 · GeekNews · [원문 보기](https://news.hada.io/topic?id=37191&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://research.google/blog/advancing-science-engineering-with-antigravitys-teamwork-feature/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Google과 학계 연구진이 Gemini 4(Argon·Carbon·Hydrogen)와 Antigravity의 /teamwork 멀티에이전트 시스템으로 수학 난제 증명, 분자 구조 규명, Rust 재작성, 인프라 최적화에서 성과를 냈다고 밝혔다. 병렬 탐색과 반증 수단(Lean 4, 원본 테스트 등), 에이전트별 문맥 분리가 핵심 설계다.

## 핵심

- /teamwork 는 병렬 탐색과 전용 반증 수단(Lean 4 증명 검사기, 수정하지 않은 테스트, 실행 시뮬레이터, 웹 검색, 하드웨어 카운터)을 결합한다. 각 전문 에이전트에는 해당 단계에 필요한 파일과 도구만 주고, 비평자·감사자는 결과를 만든 에이전트의 추론을 보지 않은 새 문맥에서 검토한다.
- 2013년 Courtade–Kumar 추측을 일반적인 경우까지 증명했다. CUHK 연구진이 문제를 14변수 최적화로 환원했고, Gemini 4 Argon 등이 해석적 경계와 구간 산술 인증서 대부분을 개발했다. 전체 증명은 Lean 4로 약 5,000만 줄 규모로 형식화했다.
- /teamwork-LEAP 은 하위 에이전트 28,800회 호출로 약 180만 줄의 Lean 코드를 생성해 Weil 추측을 처음부터 끝까지 자율적으로 형식화했다. Hilbert의 열 번째 문제(일반 정수환), 3D Navier–Stokes 유한 시간 발산, Birkhoff 추측, 3D Bochner–Riesz 추측의 형식화도 보고했다. Bochner–Riesz에서는 최근 비형식적 증명의 빈틈을 찾아 수정했다.
- Deep Review 는 STOC 2027 제출 논문 578편(전체의 3분의 1 이상)에 초안 피드백을 제공했다. 진행 중인 설문(N=70)에서 90%가 피드백이 대부분 사실에 부합한다고 답했고, 30% 이상이 실질적 증명 오류나 중요한 오탈자를 발견했다고 답했다.
- NMR 데이터 기반 분자 구조 규명에서는 30개 에이전트와 물리 정보 반영 스펙트럼 시뮬레이터를 자기 교정 루프로 묶어 top-1 정확 일치율 95%, top-3 100%를 달성했다. 단일 에이전트의 거의 두 배다.
- Zircon 마이크로커널의 남은 C++를 2주 만에 약 80만 줄의 Rust로 재작성해 모든 커널 단위 테스트를 통과하고 arm64에서 부팅했다. libgav1(AV1 디코더)은 약 17만 줄의 Rust로 옮겨 비트 단위 100% 일치했고, 최적화 후 x86_64와 ARM에서 원본보다 20~30% 빨라졌다.
- Google 프로덕션 인프라에서는 서버군 메모리 최적화로 초기 예상치 1 PiB를 넘는 절감 기회를 찾았다. Borg 스케줄링 변경 2개를 배포해 일부 하드웨어 유형에서 TPU 워크로드 선점을 24% 줄였다. 학술 연구자 1만 명에게 최상위 등급을 6개월간 무료로 제공하는 신청도 받는다.

## 자세히

/teamwork 는 워크플로를 선언적 자연어 패턴으로 표현하고, 사용자 요청을 적합한 패턴(분산 코딩, 반복 문제 해결, 긴 증명, 문서 검토 등)으로 동적 배정한다. /boost 는 반복 코딩 패턴을 독립 명령으로 낸 것이다. 장기 실행 기록을 분석해 결합도가 높은 코드는 병렬 분할보다 순차 구현과 수정이 적합하다는 결과를 얻고 /boost 를 개선했다. 팀이 과거 실행 기록으로 맞춤 명세를 만들 수도 있다. 일부 기능은 이미 제공 중이고, 재작성 패턴과 형식 증명 패턴 등은 향후 업데이트로 공개할 예정이다.

코드 이식에서는 에이전트가 의존성을 임시 대체물로 바꾸거나 테스트를 고쳐 잘못된 이식을 통과시키는 문제가 있었다. 재작성 패턴은 의존성이 컴파일된 뒤에만 해당 모듈을 처리하고, 모든 말단 모듈을 원본의 수정하지 않은 테스트에 연결해 이를 막는다. TPU 커널에서는 전체 하드웨어 평가가 7~32분 걸리는 제약 때문에 23초에 도는 대리 평가기('yardstick')를 만들고, 주기적 전체 실행과 Kendall 순위 상관으로 검증했다. JAXBench 커널 17개에서 기하평균 4.09배, vLLM TPU-Inference Pallas 커널 7개에서 기하평균 2.99배 가속을 보고했다.

기타 보고 사례로는 FarArray 기반 락 없는 연결 요소 탐색(간선 1조 개까지 확장), 9×9 바둑 엔진의 자율 hill-climbing(8일 미만에 약 2,600 Elo 상승해 1,885 Elo), 에이전트 29개가 11.5시간 동안 공개 표 13개를 50개 과제로 바꾼 인과 추론 벤치마크 생성이 있다. 이 벤치마크 생성에서는 독립 검토자와 감사자가 편법, 문구 속 정답 단서, 환각으로 만든 스크립트를 적발해 상시 테스트로 전환했다. 이 글은 Google Research 블로그(Vahab Mirrokni, 2026-11-13)로, 결과 대부분이 자사 발표이며 원문 안에서 외부 검증 내용은 확인되지 않는다. 또 Tarjan의 Deque 추측은 완전한 해결이 아니라 새 상한을 증명한 것이다.

## 왜 골랐나

AI 에이전트 개발 관점에서 문맥 분리, 생성자와 검토자의 분리, 수정 불가한 외부 검증기(원본 테스트·Lean)로 에이전트의 편법을 막는 설계가 직접 참고할 만하다. 기술경영·기술정책 연구에서는 AI 에이전트가 연구 수행과 논문 검토(STOC 2027 제출의 3분의 1 이상이 Deep Review 이용)에 쓰이는 사례로 볼 수 있다.
