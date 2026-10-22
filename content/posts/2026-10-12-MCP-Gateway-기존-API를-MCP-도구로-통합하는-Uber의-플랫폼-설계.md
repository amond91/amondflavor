---
title: "MCP Gateway - 기존 API를 MCP 도구로 통합하는 Uber의 플랫폼 설계"
date: "2026-10-12"
description: "Uber는 에이전트와 기존 백엔드·네이티브 MCP 서버 사이에 공통 계층인 MCP Gateway를 구축해, 현재 800개 이상의 MCP 서버와 5,000개 이상의 도구를 운영한다. 기존 API를 수정 없이 MCP 도구로 노출하고, 접근 제어·민감정보 제거·소유 팀 승인을 중앙에서 적용하며, 컨텍스트 비용은 Omni MCP·Response Projection·Code Mode로 줄인다."
category: "AI 에이전트·모델"
tags: ["MCP-Gateway", "Uber", "AI-에이전트", "MCP-Registry", "Omni-MCP", "Code-Mode"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35213"
original: "https://www.uber.com/in/en/blog/designing-mcp-gateway/"
coverage: "full"
---

> 📌 2026-10-12 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35213) · [원문 기사](https://www.uber.com/in/en/blog/designing-mcp-gateway/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Uber는 에이전트와 기존 백엔드·네이티브 MCP 서버 사이에 공통 계층인 MCP Gateway를 구축해, 현재 800개 이상의 MCP 서버와 5,000개 이상의 도구를 운영한다. 기존 API를 수정 없이 MCP 도구로 노출하고, 접근 제어·민감정보 제거·소유 팀 승인을 중앙에서 적용하며, 컨텍스트 비용은 Omni MCP·Response Projection·Code Mode로 줄인다.

## 핵심

- 구조는 제어 평면인 MCP Registry(서버·도구 카탈로그, 검색·소유권·활성화의 단일 기준점)와 데이터 평면인 Proxy Gateway(MCP 요청을 HTTP, gRPC, TChannel로 변환해 백엔드에 전달하고 응답을 MCP 호환 결과로 되돌림)로 나뉜다. 하위 서비스는 변경하지 않는다.
- AutoCrawler는 Cadence 기반 분산 워크플로 시스템이다. Uber의 IDL 레지스트리(Protobuf/Thrift)를 정기 스캔해 가상 MCP 서버와 도구를 만든다. LLM이 스키마와 주석을 바탕으로 에이전트 친화적인 도구 설명을 생성하고, 모든 도구는 기본 비활성 상태로 등록된다. 네이티브 MCP 서버(MCPFx 사용)는 하트비트 지표로 발견하고 listTools를 호출해 도구를 가져온다.
- '발견이 곧 노출을 뜻하지 않음'이 기본 원칙이다. 모든 서버와 도구는 소유 팀이 검토·수정한 뒤 명시적으로 활성화해야 한다. 도구 설명이 바뀔 때마다 설정 변경 diff가 생성되고 서버 소유자의 승인이 필요하며, 이전 검증 버전으로 롤백할 수 있다.
- 보안은 Uber의 Access Control System으로 호출 주체(사람, 서비스, 에이전트)별 charter 정책을 서버 수준에서 적용하고 필요하면 도구별로 재정의하는 방식이다. 도구 응답의 개인정보·민감정보는 기본적으로 제거된다. 서드파티 MCP(Jira, Google 등)는 내부 사용자 토큰을 외부 인증 토큰으로 교환해 연결한다.
- 런타임 설정은 제어 평면에서 주기적으로 받아 메모리 내 상태를 갱신하므로 재시작·재배포 없이 반영된다. 서버마다 /<service-name>/mcp 엔드포인트가 노출된다. 실제 하위 호출은 서비스 메시 사이드카 Muttley를 통한다.
- 규모에서 생기는 컨텍스트 낭비는 세 가지로 대응한다. Omni MCP는 단일 프록시로 discover_server → discover_tools → get_tool_schema → invoke_tool 순서의 단계적 탐색을 제공한다. Response Projection은 GraphQL처럼 LLM이 필요한 필드의 중첩 경로를 지정하면 게이트웨이가 응답을 잘라낸다. Code Mode는 aifx CLI(mcp list/search/call)로 출력을 파일에 저장하고 grep으로 필요한 부분만 컨텍스트에 올리며, 현재 Uber 코딩 에이전트의 기본 방식이다.
- 설계 원칙은 '기존 API가 에이전트에 도구를 제공하는 가장 빠른 경로'이다. 서비스를 재작성하지 않고 프로토콜 변환으로 연결해, 어느 팀이든 수분 내 연결할 수 있는 플랫폼을 만들었다고 한다.

## 자세히

배경은 수백 개 팀이 각자 MCP를 연동하면서 도구 파편화와 인프라 중복이 생기고, 도구를 발견하기도 안정적으로 운영하기도 어려웠으며 특정 서비스나 에이전트 구현에 강하게 결합된 문제였다. Uber는 이를 통합 없이 확장하면 운영 복잡성, 보안 위험, 개발 마찰이 커진다고 보았다.

설계의 핵심은 자동화와 거버넌스의 균형이다. AutoCrawler가 수천 개 서비스를 서비스 팀의 작업 없이 탐색해 서버·도구를 만들되, 노출 결정권은 소유 팀에 둔다. 도구 설명 변경 때마다 diff 승인과 롤백이 가능하다는 점이 이를 뒷받침한다.

글은 대규모 에이전트 시스템의 어려움이 AI 자체보다 발견, 보안, 신뢰성을 뒷받침하는 연결 기반을 만드는 데 있다고 결론짓는다. 원문은 Uber 엔지니어링 블로그(2026-10-01)의 자체 설명이며, 비용 절감이나 사용량에 대한 정량 효과는 제시되지 않았다. 800개 서버와 5,000개 도구 외의 수치는 없다.

## 왜 골랐나

AI 에이전트 개발 관점에서 도구 레지스트리, 기본 비활성과 소유자 승인 거버넌스, 단계적 도구 탐색, 응답 필드 투영으로 컨텍스트를 줄이는 패턴이 직접 참고가 된다. 기술경영 관점에서는 기존 자산(API)을 재작성하지 않고 플랫폼 계층으로 재활용하는 전략 사례로 볼 수 있다.
