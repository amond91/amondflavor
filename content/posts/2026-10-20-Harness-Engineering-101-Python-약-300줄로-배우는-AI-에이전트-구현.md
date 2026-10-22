---
title: "Harness Engineering 101 - Python 약 300줄로 배우는 AI 에이전트 구현"
date: "2026-10-20"
description: "Harness Engineering 101은 프레임워크 없이 순수 Python으로 약 300줄짜리 AI 에이전트를 키워가며, 에이전트가 'LLM API는 무상태이므로 매 턴 대화 JSON 배열을 보내는 루프'에 대한 일련의 패치임을 16개 장으로 설명하는 입문서(블로그 연재를 책으로 묶음)다."
category: "AI 에이전트·모델"
tags: ["하네스-엔지니어링", "AI-에이전트", "에이전트-루프", "컨텍스트-관리", "MCP", "Python"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35651"
original: "https://github.com/IsuruMaduranga/harness-engineering-101"
coverage: "partial"
---

> 📌 2026-10-20 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35651) · [원문 기사](https://github.com/IsuruMaduranga/harness-engineering-101)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Harness Engineering 101은 프레임워크 없이 순수 Python으로 약 300줄짜리 AI 에이전트를 키워가며, 에이전트가 'LLM API는 무상태이므로 매 턴 대화 JSON 배열을 보내는 루프'에 대한 일련의 패치임을 16개 장으로 설명하는 입문서(블로그 연재를 책으로 묶음)다.

## 핵심

- 핵심 관점은 'LLM은 두뇌, 하네스는 몸'이다. LLM API는 무상태라 매 턴 전체 대화를 JSON 배열로 보내고 텍스트를 돌려받으며, 하네스는 이 배열을 만들고 유지하고 보호하는 프로그램이다.
- 에이전트의 여러 기능은 이 기본 루프에 대한 수정(patch)이며, 각 수정은 구체적인 실패를 해결하려고 추가된다. 모든 장은 '실패 → 최소한의 수정 → 수정의 비용' 순서로 구성된다.
- 1부(1~5장)는 통신과 기본 루프를 다룬다: JSON 배열, 모델이라는 블랙박스, 도구 호출, 에이전트 루프, 캐싱(배열을 반복 전송하며 커지는 비용과 순서의 중요성).
- 2부(6~9장)는 장시간 실행 문제를 다룬다: 컨텍스트는 '가방이 아니라 예산', 하위 에이전트로 컨텍스트 분기, 실행 중 루프 조정, 백그라운드 작업과 시간.
- 3~4부(10~15장)는 생태계와 신뢰를 다룬다: 프레임워크는 1장을 감싼 래퍼, MCP·Skills·지연 로딩·훅, 배열 디버깅, 가드레일, 코딩 에이전트 사례 연구, RAG도 처음부터 하네스 패턴이었다는 관점. 16장은 주석이 달린 완성형 하네스다.
- 부록은 모델 라우팅·프롬프트 계층·비용 엔지니어링, Worktree 격리, 계획 모드, 재시도·요청 제한·스트리밍을 다룬다.
- Python 3.10 이상에서 외부 패키지 없이 실행되며 기본 예제에는 Anthropic API 키가 필요하다. 본문은 CC BY-NC-ND 4.0, 코드는 MIT 라이선스다.

## 자세히

저장소(버전 1.1, 2026년 10월)는 mdBook으로 구성되며, src/harness/에 단계별 실행 가능한 하네스, src/diagrams/에 다이어그램 생성기와 PNG가 있다. 인라인 Mermaid 다이어그램을 렌더링하려면 mdbook-mermaid가 필요하고, 없으면 원시 코드로 표시된다. 저장소는 29 커밋, 460 stars, 42 forks 상태다.

저자는 WSO2 MI Copilot, WSO2 BI Copilot, One Code를 만든 경험에서 각 장을 직접 썼다고 밝힌다. Claude Code와 One Code는 장의 순서를 결론 우선(Minto pyramid)으로 재구성하고, 영어 다듬기, 다이어그램 및 차트 코드 작성에 사용했다고 한다. 같은 아이디어를 pi 기반 오픈소스 코딩 에이전트 One Code에도 적용했다.

라이선스상 본문은 출처를 표시해 공유할 수 있지만 상업적 사용이나 수정본 배포는 불가하고, 하네스·다이어그램의 Python 코드와 본문 속 해당 코드 예제는 출처 표시 조건으로 자유롭게 쓸 수 있다.

## 왜 골랐나

AI 에이전트 개발 관심사와 직접 맞닿는다. 컨텍스트 예산 관리, 하위 에이전트, MCP·Skills·훅, 가드레일 등 에이전트 하네스 설계 요소를 최소 구현으로 학습할 수 있다.
