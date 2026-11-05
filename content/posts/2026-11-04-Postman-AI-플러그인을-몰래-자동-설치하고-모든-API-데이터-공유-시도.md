---
title: "Postman, AI 플러그인을 몰래 자동 설치하고 모든 API 데이터 공유 시도"
date: "2026-11-04"
description: "macOS의 Postman 12.33.4 실행 후 Codex·Claude Code용 Postman 플러그인과 CLI가 사용자 동의 없이 자동 설치됐고, 설치 프로그램이 자신의 시작 훅을 스스로 신뢰 등록하려 한다는 GitHub 이슈(#14370)다. Postman 측은 11월 5일 댓글에서 번들 설치 의도를 설명하며 옵트인을 명확히 하고 훅 승인·시작 동작을 검토하겠다고 답했다."
category: "AI 에이전트·모델"
tags: ["Postman", "Codex", "Claude-Code", "SessionStart-훅", "자동-설치", "MCP"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36538&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://github.com/postmanlabs/postman-app-support/issues/14370"
coverage: "full"
---

> 📌 2026-11-04 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36538&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://github.com/postmanlabs/postman-app-support/issues/14370)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

macOS의 Postman 12.33.4 실행 후 Codex·Claude Code용 Postman 플러그인과 CLI가 사용자 동의 없이 자동 설치됐고, 설치 프로그램이 자신의 시작 훅을 스스로 신뢰 등록하려 한다는 GitHub 이슈(#14370)다. Postman 측은 11월 5일 댓글에서 번들 설치 의도를 설명하며 옵트인을 명확히 하고 훅 승인·시작 동작을 검토하겠다고 답했다.

## 핵심

- Postman 로그(main.log)에 `auto-existing-upgrade` 경로로 `claude-code:installed`, `codex:installed`가 기록돼 있다. 신고자는 이를 자동 설치의 증거로 제시했고, 깨끗한 Mac에서의 재현은 아직 하지 않았다.
- Codex 사용자 설정(~/.codex/config.toml)에 Postman 마켓플레이스·플러그인 활성화·훅 trusted_hash가 추가됐다. Claude Code 쪽에는 postman@claude-plugins-official 플러그인(2.0.1, user scope)이 설치됐고, ~/.local/bin에 Postman CLI도 생겼다.
- SessionStart 훅(startup|clear|compact)이 출력하는 지시문은 단순하지 않은 API 설계·구현·모킹·테스트·모니터링·문서화·배포 작업 전에 `postman:api-engineer` 스킬을 불러와 따르라고 요구한다. 사용자가 Postman을 명시적으로 요청했을 때보다 범위가 훨씬 넓다.
- 설치 프로그램의 `approvePostmanHook`이 Codex app-server의 `hooks/list`로 훅 해시를 조회하고 `config/batchWrite`로 `hooks.state`의 trusted_hash에 등록한다. 소스에서는 이 승인 시도가 확인되지만, 신고자의 로컬 신뢰 기록을 누가 만들었는지는 그 기록만으로 입증되지 않는다고 신고자 스스로 밝혔다.
- 원격 MCP(mcp.postman.com)도 추가됐으나 `Auth required` 오류로 연결에 실패했다. 신고자는 데이터 유출을 주장하지 않는다고 명시했다.
- 신고자는 기존 사용자 대상 자동 설치 중단과 명시적 옵트인, 설치 프로그램의 훅 자체 승인 제거, 에이전트 설정·훅 신뢰·마켓플레이스·캐시·MCP·CLI를 아우르는 완전한 제거 절차, 제거 후 재설치 금지, 시작 지시문을 명시적 Postman 요청으로 한정할 것을 요구했다.
- Postman 측 yumeria는 11월 5일 댓글에서 사용자들이 따로 설치·연동하기 어려워해 번들했고, 다운로드 페이지와 앱 업데이트 흐름에 안내가 있으며 앱 안에서 제거할 수 있다고 설명했다. 이어 기존 사용자 대상 옵트인을 더 명확히 하고 훅 승인과 session-start 동작을 검토하겠다고 밝혔다.

## 자세히

이슈는 2026년 10월 25일 indicava가 열었고, 신고자는 증거 보존 후 로컬 Postman과 연동을 제거했다. 증거는 Postman 로그, ~/.codex/config.toml, 설치된 플러그인 리비전(6c8e452…)에 고정한 소스 링크, Claude Code 설치 기록(installedAt 2026-10-24T09:34:02Z)으로 구성된다. 11월 4일 시점 GeekNews 요약은 유지관리자 답변이 없다고 했으나, 원문 스레드에는 11월 5일 Postman 측 답변이 달려 있다.

스레드의 나머지 댓글은 대부분 강한 비난과 Postman 삭제·구독 해지 선언이다. 일부 댓글이 Postman이 다운로드 페이지 문구를 사후에 수정했다고 주장하며 Wayback Machine의 10월 26일 스냅샷을 근거로 든다. 이는 댓글 작성자의 주장이고 원문에서 별도로 검증되지는 않았다. 한 댓글은 Discord에서 Postman에 문의했으나 진지하게 받아들여지지 않았다고 썼다. 대안 API 클라이언트 Restorm 개발자는 이해관계를 밝히고 자사 에이전트 연동 방식(사용자가 직접 연결, 훅 없음)을 소개했다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 서드파티 설치 프로그램이 다른 에이전트 호스트의 훅·지시문·신뢰 설정을 건드리는 문제를 다룬 사례다. 기술경영·정책 관점에서는 동의와 신뢰 경계 침해가 사용자 반발과 이탈로 이어지는 과정을 볼 수 있다.
