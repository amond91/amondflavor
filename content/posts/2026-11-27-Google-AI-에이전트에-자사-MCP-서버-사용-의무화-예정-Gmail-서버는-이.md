---
title: "Google, AI 에이전트에 자사 MCP 서버 사용 의무화 예정… Gmail 서버는 이메일 발송 불가"
date: "2026-11-27"
description: "Google은 2027년 2월 1일부터 AI 에이전트(또는 AI 모델이 구동하는 도구)가 Workspace에 접근할 때 자사 MCP 서버를 쓰도록 약관으로 의무화한다. 그런데 공개 Gmail MCP 서버는 발송·답장·전달·초안 수정이 파트너 전용이고, 임베딩·메모리 인덱스 보존도 30일로 제한될 수 있어 독립 개발자에게 불리한 구조라는 분석이다."
category: "AI 에이전트·모델"
tags: ["Google-Workspace", "MCP", "AI-에이전트", "Gmail", "플랫폼-통제", "데이터-보존-30일"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=37919&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://techstackups.com/articles/google-workspace-ai-agents-mcp/"
coverage: "full"
---

> 📌 2026-11-27 · GeekNews · [원문 보기](https://news.hada.io/topic?id=37919&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://techstackups.com/articles/google-workspace-ai-agents-mcp/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Google은 2027년 2월 1일부터 AI 에이전트(또는 AI 모델이 구동하는 도구)가 Workspace에 접근할 때 자사 MCP 서버를 쓰도록 약관으로 의무화한다. 그런데 공개 Gmail MCP 서버는 발송·답장·전달·초안 수정이 파트너 전용이고, 임베딩·메모리 인덱스 보존도 30일로 제한될 수 있어 독립 개발자에게 불리한 구조라는 분석이다.

## 핵심

- 11월 24일 Google이 Workspace MCP 서버 8개를 정식 출시하고 API 약관에 Workspace AI Tools Addendum을 추가했다. 2027년 2월 1일부터 AI 에이전트·AI 하네스·AI 모델 기반 도구는 Google MCP를 써야 하고, 에이전트임을 숨기는 행위도 금지된다. 위반하면 접근 제한·제거나 계정 정지가 가능하다.
- 같은 날부터 REST API를 직접 호출하는 에이전트에는 유료 등급이어도 Standard 등급 할당량이 적용된다. REST 호출은 계속 작동하지만 약관 위반이 된다. AI를 쓰지 않는 백업·CRM 동기화·결정론적 스크립트는 별도 OAuth 클라이언트를 쓰면 영향이 없다.
- 공개 Gmail MCP는 검색·읽기·초안 생성·라벨·휴지통·스팸만 지원한다. 발송·답장·전달·초안 수정은 파트너 접근이 필요하며, 파트너 자격 기준과 일반 공개 시점은 공개되지 않았다. Drive도 이동·이름 변경·공유·휴지통 이동·댓글이 빠져 있다. Docs·Slides·Tasks는 REST와 거의 같은 수준이다.
- Workspace 데이터는 진행 중인 사용자 승인 작업에만 처리할 수 있고 연결 해제 시 삭제해야 한다. Google은 벡터 임베딩·메모리 인덱스·사용자 프로필·조직 그래프 등 'Google Technical Information'의 보존 기간을 30일로 제한할 수 있다. 이 데이터로 학습·미세 조정·평가·증류를 하거나 경쟁 Workspace 제품을 만드는 것도 금지된다.
- 무료 Standard 등급은 프로젝트·제품별 하루 5,000 MCP 요청이다. Gmail search_threads와 list_drafts는 각각 10회분으로 계산되어, 검색만 하면 프로젝트 전체에서 하루 500회가 한도다. 유료 Scaled 등급은 2027년 4월 1일부터 Gmail·Calendar·Drive에서 과금을 시작하며 MCP는 첫 요청부터 과금한다. REST는 Gmail 가벼운 요청 기준 하루 7,000만 건 무료 할당량이 있고, 요청당 가격은 미공개다.
- 에이전트 분류는 OAuth 클라이언트의 'Use this client for an AI-powered agent' 체크박스 자기 신고로 시작한다. 11월 11일부터 MCP 서버는 표시 없는 클라이언트를 거부하며, 기준일 전에 표시하지 않은 클라이언트는 영구히 비에이전트 상태라 새로 만들어야 한다. 도메인 전체 위임 서비스 계정과 Apps Script 내장 클라이언트는 에이전트로 표시할 수 없다.
- Google은 근거로 '유효한 OAuth 토큰만으로는 사용자 의도를 보장할 수 없다'는 점과 프롬프트 인젝션 위험을 든다. MCP 게이트웨이는 관리자가 작업 범주 차단·승인·감사·DLP를 한곳에서 적용하게 해 준다. 다만 같은 구조가 Google에 에이전트별 기능과 데이터 보존 범위를 통제할 권한도 준다. 원문은 Slack이 2025년 5월 도입한 유사 제한과 비교한다.

## 자세히

원문(techstackups.com)은 이 정책을 보안 강화와 플랫폼 통제라는 두 면으로 본다. 에이전트가 기계 속도로 자율 행동하므로 요청 경로에 정책 검사와 사람의 확인을 넣어야 한다는 Google의 논리에는 실질이 있다고 인정한다. 그러면서도 파트너 전용 도구와 30일 임베딩 제한이 그 통제가 실제로 쓰인 사례라고 지적한다.

파트너와 독립 개발자 사이의 격차도 짚는다. Anthropic의 Claude용 Google Workspace 커넥터는 이메일 발송·답장·전달과 Drive 파일 이동·휴지통 이동을 지원한다고 안내하는데, 공개 MCP 서버에는 없는 작업이다. 같은 OAuth 범위를 받아도 독립 개발자는 초안만 남길 수 있고, 약관이 발효되면 REST로 보완하는 것도 위반이 된다.

대상별 권고는 다음과 같다. 에이전트 제품 개발자는 에이전트로 표시한 OAuth 클라이언트를 만들어 MCP로 이전하고, 부족한 기능은 파트너 접근을 일찍 신청하며, 임베딩·메모리 인덱스의 보존 기간을 점검해야 한다. 개인 에이전트 운영자의 google_workspace_mcp 같은 REST 기반 오픈소스 서버는 계속 작동하지만 Standard 할당량과 약관 위반 상태가 된다. Google 서버로 바꾸면 Gmail 발송·초안 수정과 Drive 이동·공유를 잃는다. Workspace 관리자는 관리 콘솔 Security → API Controls에서 정책을 설정하고 감사 로그를 볼 수 있다. 사용자는 저장된 승인을 조회·취소할 수 없고 승인은 90일 후 만료된다. 에이전트 탐지 방식과 취미 목적 개인 사용에 대한 적용 여부는 Google이 밝히지 않았다.

## 왜 골랐나

AI 에이전트 개발 관점에서 직접 관련된다. Gmail·Drive를 다루는 에이전트를 만든다면 2027년 2월 전에 MCP 이전, 필요한 기능이 빠졌는지, 임베딩 보존 기간을 점검해야 한다. 기술경영·기술정책 연구 관점에서는 플랫폼 사업자가 보안을 명분으로 파트너 등급과 데이터 사용 약관으로 생태계를 통제하는 사례로 볼 수 있다. 원문은 Slack의 2025년 5월 조치를 비교 사례로 든다.
