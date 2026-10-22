---
title: "Show HN: Cloudflare에서 무료로 실행하는 WhatsApp MCP"
date: "2026-10-19"
description: "whatsapp-mcp는 개인 Cloudflare 계정에 배포해 AI 에이전트(MCP 클라이언트)가 자신의 WhatsApp 대화를 읽고 검색하게 하는 읽기 전용 서버로, 비공식 Baileys 라이브러리를 쓰기 때문에 WhatsApp 약관에 어긋납니다. 제목의 '무료 실행'과 달리 README는 초기 기록 가져오기의 CPU 한도 때문에 Workers Paid 플랜(월 몇 달러)을 요구합니다."
category: "AI 에이전트·모델"
tags: ["MCP", "WhatsApp", "Cloudflare-Workers", "Durable-Object", "Baileys", "AI-에이전트"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35643"
original: "https://github.com/finereli/whatsapp-mcp"
coverage: "partial"
---

> 📌 2026-10-19 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35643) · [원문 기사](https://github.com/finereli/whatsapp-mcp)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

whatsapp-mcp는 개인 Cloudflare 계정에 배포해 AI 에이전트(MCP 클라이언트)가 자신의 WhatsApp 대화를 읽고 검색하게 하는 읽기 전용 서버로, 비공식 Baileys 라이브러리를 쓰기 때문에 WhatsApp 약관에 어긋납니다. 제목의 '무료 실행'과 달리 README는 초기 기록 가져오기의 CPU 한도 때문에 Workers Paid 플랜(월 몇 달러)을 요구합니다.

## 핵심

- 휴대폰을 WhatsApp Web처럼 '연결된 기기'로 한 번 연결하면, Claude Code·Claude Desktop 등 MCP 클라이언트가 대화 목록 조회(list_chats), 대화 읽기(get_messages), 전체 검색(search_messages)을 할 수 있습니다. 메시지 전송은 의도적으로 구현하지 않았습니다.
- 상시 연결이 아니라 에이전트 요청 시와 매일 UTC 04:00에 접속해 대기 중인 데이터를 수집한 뒤 연결을 끊습니다. 저장본이 10분 이상 오래되면 읽기 도구가 먼저 동기화합니다. 데이터는 자신의 Cloudflare 계정 Durable Object 안 SQLite(FTS5 전문 검색 색인 포함)에만 저장됩니다.
- WhatsApp 공식 API가 없어 Baileys(WhatsApp Web 프로토콜)를 사용하며, 이는 WhatsApp 약관에 어긋납니다. 읽기 전용 연결 기기의 실질적 위험은 낮지만 0은 아니라고 안내합니다. 최초 연결 때 WhatsApp이 넘겨주는 기록만 가져오므로 전체 기록이 아닐 수 있고, 기록 전달은 연결당 한 번뿐입니다.
- 미디어는 내려받지 않고 [image], [voice note 12s] 같은 표시와 캡션으로만 나타나며, 음성 변환도 없습니다. 연락처 이름이 없는 대화는 전화번호로 보이는데, unnamed_chats와 set_contact_names로 에이전트가 Google Contacts·Gmail 커넥터 등 기존 연락처 도구를 통해 이름을 채웁니다. 서버가 연락처 서비스에 직접 로그인하지 않는 설계입니다.
- 배포 하나당 WhatsApp 계정 하나와 소유자 한 명만 지원합니다. 각 배포가 전체 메시지 기록과 유효한 로그인 정보를 보유하기 때문입니다. 라이선스는 MIT이며, 실제 사용 검증은 계정 하나에서만 진행됐습니다.
- Baileys를 Cloudflare workerd에서 돌리려면 호환 계층 세 가지(ws-shim, wasm 추출 vendor-bridge, .unref() 폴리필)가 필요하고, lru-cache의 ttlAutopurge가 타이머 10,000개 한도를 넘겨 최초 가져오기를 실패시키는 문제는 lru-shim으로 우회합니다. Baileys는 사전 릴리스라 정확한 버전 고정이 필요합니다.
- 알려진 한계로, 일일 수집 중 로그아웃 상태를 발견해도 알림이 없고 에이전트가 link_status를 조회해야 알 수 있습니다. 메시지 수정·삭제 반영도 실제 이벤트로는 시험하지 않았습니다.

## 자세히

구조는 상태 없는 Worker(OAuth 토큰과 단기 로그인 기록만 KV에 보관)와 단일 Durable Object(WhatsAppStore, 이름 'owner')로 나뉩니다. Worker가 /mcp 요청마다 새 McpServer를 만들고 RPC로 Durable Object를 호출하며, Baileys WebSocket은 연결 등록이나 수집 세션 동안에만 열립니다. MCP 클라이언트는 /mcp 엔드포인트와 OAuth로 접근하고, 기본 소유자 인증은 12자 이상 비밀번호입니다.

데이터 적재는 멱등적이라 기록을 다시 가져와도 중복이 생기지 않고 누락된 sender나 push_name만 채웁니다. 그래서 기존 메시지를 유지한 재연결이 가능합니다. 표시 이름은 WhatsApp 대화 이름, WhatsApp 연락처 이름, 별도 저장 이름, 상대방 표시 이름, 전화번호 순으로 정해지며, 익명 @lid 식별자는 전화번호 JID로 매핑해 한 사람이 한 대화로 남게 합니다.

운영 측면에서는 활성 세션이 있는 Durable Object가 배포 후에도 이전 코드를 계속 실행할 수 있어, 수정 배포 후 문제가 지속되면 요청을 멈추고 3~5분 기다려야 합니다. 디버그 기능은 PIN과 x-pin 헤더로 보호되는 선택 기능입니다. 비용 면에서 한도 상향은 월 몇 달러 수준의 유료 플랜이 필요하며, 무료 플랜은 시험하지 않았고 limits 블록을 제거하면 최초 가져오기가 불안정할 수 있다고 합니다.

원문 기사(GitHub README)는 끝부분이 로컬 개발 설명 도중 잘려 있습니다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 읽기 전용 도구 설계, 실패 결과에 다음 행동을 안내하는 방식, 외부 연락처 인증을 서버가 아닌 에이전트에 위임하는 MCP 설계 사례로 참고할 만합니다.
