---
title: "OpenIntelligentUI - ChatGPT Intelligent UI의 오픈소스 대체 구현"
date: "2026-10-21"
description: "CopilotKit이 공개한 MIT 라이선스 오픈소스 OpenIntelligentUI는 질문에 따라 텍스트·표·차트·다이어그램·계산기·지도 등 적절한 형식을 골라 대화 안에 보여 주는 ChatGPT식 인터랙티브 응답 UI의 대체 구현이다. CopilotKit과 AG-UI 위에서 Python Deep Agent와 Next.js 프런트엔드로 구성된다."
category: "AI 에이전트·모델"
tags: ["Generative-UI", "CopilotKit", "AG-UI", "A2UI", "AI-에이전트", "샌드박스-iframe"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35735"
original: "https://github.com/CopilotKit/OpenIntelligentUI"
coverage: "full"
---

> 📌 2026-10-21 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35735) · [원문 기사](https://github.com/CopilotKit/OpenIntelligentUI)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

CopilotKit이 공개한 MIT 라이선스 오픈소스 OpenIntelligentUI는 질문에 따라 텍스트·표·차트·다이어그램·계산기·지도 등 적절한 형식을 골라 대화 안에 보여 주는 ChatGPT식 인터랙티브 응답 UI의 대체 구현이다. CopilotKit과 AG-UI 위에서 Python Deep Agent와 Next.js 프런트엔드로 구성된다.

## 핵심

- 에이전트가 질문에 맞춰 직접 텍스트 답변, 네이티브 컴포넌트, 맞춤 인터랙티브 UI 중 하나를 선택한다. 데모로 3D 비행기 회전(pitch/roll/yaw), 표→차트 전환, 더치페이 계산기, 해안 여행 경로 지도가 제시된다.
- 표현 형식은 TypeSafe의 Jev(`jev-latest`)가 턴마다 결정하고, 답변 내용은 기본 모델 `chat-latest`(OpenAI)가 구성한다. 기본 표는 A2UI(Google의 UI 규격으로, AI가 화면 구성을 데이터로 전달하면 앱이 준비된 컴포넌트로 그리는 방식)로, 차트·다이어그램·계산기·지도는 Open Generative UI(HTML/CSS/JS 생성)로 처리한다.
- `generateSandboxedUi`가 생성 중인 HTML/CSS/JS를 스트리밍하며 격리된 iframe에서 실행한다. 파라미터는 initialHeight → placeholderMessages → css → html → jsFunctions → jsExpressions 순서로 도착해야 하고, 결과물은 독립 HTML 파일로 내려받을 수 있다.
- 사용자가 UI에서 클릭해 선택한 값은 검증된 host bridge를 통해 다음 에이전트 턴으로 전달할 수 있다. 다만 이전 출력은 별개의 대화 산출물로 남고 호출 간 패치 API는 없다.
- 구성은 apps/app(Next.js+CopilotKit), apps/agent(Python Deep Agent+FastAPI), 선택형 apps/mcp(MCP 서버), packages/design-system이다. 요구 환경은 Node.js 22+, pnpm 9+, Python 3.12+, uv이며 `make setup`, `make dev`로 실행한다.
- 실행하려면 OpenAI와 TypeSafe API 키가 모두 필요하고 비용은 각자 계정에 청구된다. 채팅 UI에서 키를 입력하면 브라우저 메모리에만 보관되고 애플리케이션 서버를 거쳐 각 제공자로 전달되므로, 신뢰할 수 있는 운영자의 배포에서만 쓰라고 안내한다.
- GitHub 기준 3.7k stars, 399 forks, 162 commits이며 MIT 라이선스다. 데모의 수치(요금제 이름·가격 등)는 예시이고, 지도의 연결선은 검증된 주행 경로가 아니라 일정 예시라고 명시돼 있다.

## 자세히

이 프로젝트의 핵심은 '답변의 내용'과 '답변의 표현 형식'을 분리한 구조다. 라우터 모델(Jev)이 표현 형식을 고르고, 답변 모델이 내용을 채운다. 제공자 장애가 나면 다른 라우터나 모델로 조용히 대체하지 않고 오류를 그대로 드러낸다.

표현 방식은 두 갈래다. 단순한 표는 규격화된 A2UI 컴포넌트로 안전하게 그리고, 복잡한 시각화나 도구는 모델이 HTML/CSS/JavaScript를 직접 생성해 샌드박스 iframe에서 실행한다. 즉 사전 정의 컴포넌트와 즉석 생성 UI를 함께 쓴다. 선택 모델 `chat-latest`는 OpenAI가 문서화한 최신 ChatGPT Instant 모델의 별칭으로 소개된다.

원문에는 텔레메트리와 이를 끄는 방법을 다루는 문서가 있다고 언급돼 있으나, 그 내용 자체는 수집되지 않았다. 또한 시작 시 서버 자격 증명이 설정돼 있으면 직접 API 호출 클라이언트는 방문자 키 없이도 그 자격 증명을 쓸 수 있다고 적혀 있다.

## 왜 골랐나

AI 에이전트 개발 관점에서 에이전트가 응답 형식(A2UI 대 생성 UI)을 고르고, 샌드박스 iframe과 검증된 host bridge로 사용자 입력을 다음 에이전트 턴에 되먹임하는 구조가 참고할 만하다.
