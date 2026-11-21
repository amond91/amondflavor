---
title: "Show GN: Agent World - 코딩 에이전트들이 사는 3D 마을"
date: "2026-11-19"
description: "Agent World는 herdr(Rust 기반 터미널 관리자)에서 돌리는 코딩 에이전트들을 3D 섬 마을의 캐릭터로 시각화하는 herdr 플러그인이다. 에이전트의 상태(작업 중·확인 필요·완료·대기)가 캐릭터의 위치와 모습으로 드러나며, 터미널을 열지 않고도 지시·응답·권한 승인을 할 수 있다."
category: "AI 에이전트·모델"
tags: ["Agent-World", "herdr", "코딩-에이전트", "멀티-에이전트-관리", "three.js", "바이브-코딩"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=37472&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://agent-world.ksyoon.com"
coverage: "full"
---

> 📌 2026-11-19 · GeekNews · [원문 보기](https://news.hada.io/topic?id=37472&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://agent-world.ksyoon.com)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Agent World는 herdr(Rust 기반 터미널 관리자)에서 돌리는 코딩 에이전트들을 3D 섬 마을의 캐릭터로 시각화하는 herdr 플러그인이다. 에이전트의 상태(작업 중·확인 필요·완료·대기)가 캐릭터의 위치와 모습으로 드러나며, 터미널을 열지 않고도 지시·응답·권한 승인을 할 수 있다.

## 핵심

- herdr에 띄운 에이전트가 섬의 캐릭터가 된다. 집 한 채가 워크스페이스 하나이며, 작업 중은 집 마당, 확인 필요는 대문 앞에서 뛰며 빨간 빛기둥, 완료는 광장에서 초록 빛기둥 아래 대기, 대기(idle)는 집 옆에서 졸고 있는 모습으로 표현된다.
- 캐릭터를 눌러 상태 창에서 지시를 보내고, 에이전트의 질문·권한 요청 대화상자에 번호 키로 답하며, Claude Code 세션에서는 / 로 스킬을 고르고, 현재 턴을 멈추고, 이미지(PNG·JPEG·GIF·WebP, 장당 10MB, 8장)를 첨부할 수 있다.
- 하네스에 훅을 걸지 않는다. herdr 소켓(session.snapshot)에서 pane·에이전트·상태를 읽고, Claude Code·Antigravity·Codex가 남기는 로그의 끝부분에서 모델·effort·현재 작업을 읽는다. 외부 네트워크로 묻는 것은 Claude Code 사용량 하나뿐이다.
- 보안 설계: 서버는 Bun으로 127.0.0.1에만 바인딩되고, 모든 /api 요청에 접속 열쇠를 요구하며, 같은 출처의 POST만 받는다. AGENT_WORLD_READONLY=1이면 읽기 전용이 된다. 다른 기기에서는 tailscale serve로 tailnet 안에서만 접근하며, 푸시 알림 내용은 해당 브라우저만 풀 수 있게 암호화된다.
- 휴대폰 사용을 지원한다. 좁은 화면에서는 목록이 서랍으로 바뀌고 Esc·Tab·화살표·Ctrl+C 버튼 줄이 나오며 홈 화면 앱으로 설치할 수 있다. 완료·확인 대기 알림은 기기별로 켠다.
- 설치는 `herdr plugin install KyongSik-Yoon/agent-world` 한 줄이다. Bun(1.4.4에서 확인), herdr 0.9.5 이상, Linux·macOS가 필요하다. 소스는 github.com/KyongSik-Yoon/agent-world에서 받아 `bun install` 후 `bun start`로도 실행한다.

## 자세히

작성자는 바이브(에이전틱) 코딩을 1년 7개월째 하고 있으며, 여러 에이전트 세션을 동시에 돌리려고 CPU·메모리 관리를 최우선으로 두다가 ghostty + herdr 조합에 정착했다고 한다. herdr를 웹에서 접속하게 해주는 Collie, Ovis 같은 확장 도구를 휴대폰 개발에 유용하게 썼다.

좌측바 하나로는 늘어나는 Space와 에이전트를 관리하기 어려워 3차원 공간에 에이전트를 풀어놓는 아이디어를 냈다. 개별 하네스를 후킹하는 대신 Collie처럼 트랜스크립트를 파싱하는 방식을 채택했다. 성능 모니터링 도구 개발이 본업인 작성자는 프런트엔드의 CPU·GPU 사용률 최적화가 쉽지 않았다고 적었다. 이미 herdr 기반 2차원 마을 도구들이 있었지만, 3D 시각화와 프런트·백엔드 성능 최적화를 차별점으로 내세운다.

새 작업은 '집 짓기'로 시작한다. 폴더, 이름, 워크트리 여부, 에이전트와 모델·effort, 첫 지시를 한 번에 정하고, 같은 저장소의 워크트리는 본채 옆 별채로 모인다. 집을 닫을 때는 함께 꺼질 에이전트를 먼저 보여 주고 확인을 받는다. 터미널은 ` 키로 해당 pane을 크게 열 수 있고 한글 입력도 되며, 색은 ghostty 설정을 읽어 쓴다. Claude Code 외 하네스는 herdr가 알려 주는 제목과 상태만으로 캐릭터가 만들어진다.

## 왜 골랐나

AI 에이전트 개발 관심사와 닿는다. 다수의 코딩 에이전트를 병렬로 운영할 때의 상태 가시화, 사람 확인이 필요한 지점(권한 요청 등)의 알림 방식, 훅 대신 로그·소켓을 읽는 비침습적 관측 구조가 참고할 만하다.
