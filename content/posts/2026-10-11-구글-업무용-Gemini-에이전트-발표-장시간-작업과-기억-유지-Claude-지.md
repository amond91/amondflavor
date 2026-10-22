---
title: "구글, 업무용 Gemini 에이전트 발표 — 장시간 작업과 기억 유지, Claude 지원"
date: "2026-10-11"
description: "Google Cloud가 2026-10-09 Gemini at Work 2026에서 업무 전반을 맡는 단일 범용 에이전트 'Gemini agent'를 발표했다. 클라우드에서 장시간 실행되며 기기·채널이 바뀌어도 기억을 유지하고, Gemini와 Anthropic Claude 모델을 작업별로 골라 쓰며, 에이전트별 신원·권한·감사·샌드박스·지출 한도로 거버넌스를 제공한다."
category: "AI 에이전트·모델"
tags: ["Gemini-agent", "Google-Cloud", "멀티-에이전트", "MCP", "에이전트-거버넌스", "멀티모델-오케스트레이션"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35151"
original: "https://cloud.google.com/blog/products/ai-machine-learning/welcome-to-gemini-at-work-2026"
coverage: "partial"
---

> 📌 2026-10-11 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35151) · [원문 기사](https://cloud.google.com/blog/products/ai-machine-learning/welcome-to-gemini-at-work-2026)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Google Cloud가 2026-10-09 Gemini at Work 2026에서 업무 전반을 맡는 단일 범용 에이전트 'Gemini agent'를 발표했다. 클라우드에서 장시간 실행되며 기기·채널이 바뀌어도 기억을 유지하고, Gemini와 Anthropic Claude 모델을 작업별로 골라 쓰며, 에이전트별 신원·권한·감사·샌드박스·지출 한도로 거버넌스를 제공한다.

## 핵심

- Gemini agent는 질의응답, 지식 업무, 이미지·미디어 생성, 코드 작성·실행을 하나의 에이전트와 하나의 API로 처리한다. 사용자는 지시가 아니라 목표를 위임하고, 일정 예약이나 이벤트 응답도 맡길 수 있다.
- 클라우드에서 실행되어 기기·채널(웹, 모바일, 데스크톱, CLI, Workspace, Microsoft 365, Slack)과 무관하게 기억·맥락·개인화 그래프가 하나로 유지된다. 몇 시간~며칠 걸리는 작업도 노트북을 닫은 뒤 계속 실행된다. 기억은 세션·의미(semantic)·절차(procedural)·일화(episodic) 네 종류다.
- 멀티 에이전트 구조: 작업별 임시 하위 에이전트(각자 신원 보유)를 동적으로 생성하고, 지속적 역할을 가진 '동료(coworker) 에이전트'는 자체 @agents.company.com 이메일, 저장 공간, Workspace 계정(캘린더·Drive·디렉터리 표시)을 갖는다. 동료 에이전트는 공유받은 맥락에만 접근한다.
- 모델 선택: 에이전트와 모델을 분리해 Gemini 계열과 Anthropic Claude 모델 사이에서 작업별로 오케스트레이션하며, 향후 다른 비공개·개방형 모델도 지원할 예정이다. 원문은 모델 선택을 열어 두면 모델이 바뀌어도 맥락·스킬·데이터가 유지된다고 설명한다.
- 도구·스킬 확장: MCP 서버(사내외)와 Confluence, Slack, Git, Jira, Salesforce, ServiceNow, BigQuery, Snowflake 등에 연결하며, 사내 도구 레지스트리와 스킬 레지스트리로 팀이 만든 도구·절차를 공유한다. 스킬은 재사용 가능한 모듈형 프롬프트로 정의된다.
- 거버넌스: 에이전트마다 암호학적으로 증명되는 신원과 최소 권한, OAuth 기반 신원 전파, 에이전트 귀속 감사 로그를 둔다. 모든 에이전트는 자체 네트워크 경계의 Agent Sandbox에서 실행되고, 모든 트래픽은 정책을 실시간 적용하는 AI 네트워크 방화벽 Agent Gateway를 거친다.
- 비용 통제: 멀티모델 오케스트레이션, Smart Routing, 실시간 지출 한도를 제공한다. GeekNews 요약에 따르면 프로젝트 지출 한도에 도달하면 에이전트 작업이 일시 중단된다. 원문은 토큰당 가격이 2024년 이후 98% 하락했지만 사용량이 폭증했다고 설명한다.
- 그 밖의 발표: 데이터·분석 스킬(Knowledge Catalog, Smart Storage, Borderless Lakehouse), 금융·법률 특화 버전 프리뷰(정부·헬스케어·리테일은 출시 예정), Workspace 인라인 통합.

## 자세히

Workspace 안에서는 개인 비서, 선제적 위임 제안, 팀원 역할의 세 방식으로 동작한다. 예컨대 매니저가 이메일로 프로젝트 업데이트 슬라이드를 요청하면 Workspace Intelligence가 위임 가능한 작업으로 인식해 한 번의 클릭으로 Gemini에 넘길 수 있다. 동료 에이전트는 문서 댓글에서 수정을 제안하고, 버전 기록에 자기 이름으로 남는다. 원문은 접근 권한이 기존 공유·멤버십을 따르며 외부 커넥터가 데이터를 보유하지 않는다고 밝힌다.

데이터 영역에서는 Knowledge Catalog에 비즈니스 정의(예: "net margin")를 한 번 매핑해 모든 에이전트가 공유하게 하고, 저장된 쿼리는 이후 토큰 비용 없이 실행할 수 있다고 한다. Bloomberg Media는 Knowledge Catalog 기반으로 SQL 쿼리 정확도를 63% 높였다고 원문이 전한다.

고객 사례로 BNP Paribas(직원 65,000명 이상 대상 LLM@CIB), Bradesco(문서 검토 1시간→5분), SOMPO(직원 34,000명, 맞춤 에이전트 10,000개 이상), Orange Spain(맞춤 에이전트 1,000개 이상), Wesfarmers/Bunnings(관리 업무 50만 시간 절감) 등이 소개됐다. 이는 모두 Google 측 발표 수치다. 원문은 비용 통제 절의 도중에 끊겨 있어 지출 옵션의 세부 내용은 확인할 수 없다.

## 왜 골랐나

AI 에이전트 개발 관점에서 동료 에이전트의 독립 신원, 스킬·도구 레지스트리, MCP 연동, 네 종류의 기억 구조, Agent Gateway 기반 정책 통제는 에이전트 아키텍처와 거버넌스를 보여주는 사례다. 에이전트와 모델을 분리하는 설계에 Claude를 포함한 점은 기술경영 관점에서 플랫폼 전략과 모델 간 경쟁 구도를 볼 단서가 된다.
