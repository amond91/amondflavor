---
title: "Show GN: aside-relay - 휴대폰에서 Aside를 쓰는 웹 클라이언트"
date: "2026-10-11"
description: "aside-relay는 Mac에서 실행 중인 Aside 앱·CLI를 휴대폰이나 다른 브라우저에서 쓰게 해 주는 셀프호스팅 웹 클라이언트로, 기존 aside-remote를 포크해 기능과 UI를 확장했고 MIT 라이선스로 공개됐다. 대화, 브라우저 화면 확인·조작, 승인 응답, 완료 푸시 알림을 지원한다."
category: "AI 에이전트·모델"
tags: ["aside-relay", "Aside", "원격-에이전트-클라이언트", "Tailscale", "Web-Push", "셀프호스팅"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35153"
original: "https://github.com/mym0404/aside-relay"
coverage: "partial"
---

> 📌 2026-10-11 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35153) · [원문 기사](https://github.com/mym0404/aside-relay)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

aside-relay는 Mac에서 실행 중인 Aside 앱·CLI를 휴대폰이나 다른 브라우저에서 쓰게 해 주는 셀프호스팅 웹 클라이언트로, 기존 aside-remote를 포크해 기능과 UI를 확장했고 MIT 라이선스로 공개됐다. 대화, 브라우저 화면 확인·조작, 승인 응답, 완료 푸시 알림을 지원한다.

## 핵심

- 진행 중인 작업을 확인하고 추가 메시지를 보내며, 승인이 필요한 요청(approval, confirmation, question prompt)에도 응답할 수 있다. 에이전트가 쓰는 브라우저 화면을 보면서 직접 클릭·입력도 가능하다.
- 주요 기능은 실시간 스트리밍 대화와 모델·provider·reasoning effort 선택, 생성 파일·HTML 결과물 확인, 후속 메시지 큐의 편집(Edit message)·방향 수정(Steer instead)·취소, 작업 완료 푸시 알림(Web Push, 선택적 ntfy), 모바일 UI와 테마·폰트 설정이다.
- Mac에 Aside 앱과 CLI가 설치·로그인되어 있어야 하고, 원격 사용 중에는 Mac과 Aside가 계속 실행 중이어야 한다. 별도의 AI API 키는 필요 없다.
- 접속 경로는 localhost, Tailscale Serve(개인용 권장), 자체 도메인(Caddy/Nginx), LAN/VPN, Tailscale Funnel, Cloudflare Tunnel+Access 중 선택한다. Cloudflare는 선택 사항이며, 원격 사용 시 HTTPS를 권장한다.
- 인증은 브리지 자체의 access token 방식이다. 이 토큰은 연결된 Aside 계정의 에이전트 실행과 로그인된 브라우저에 대한 접근 권한을 주는 공유 자격이므로, 의도한 기기에만 전달해야 한다. 단일 계정용이며 사용자별 역할이나 토큰은 없다.
- 설치는 Python 3.11+, Node.js 22.22.2+ 등을 요구한다. 저장소를 clone해 venv를 만들고 `npm ci`, `npm run build` 후 `scripts/setup.py`로 설정한다. `server.py`를 단일 프로세스로 실행하며, LaunchAgent로 로그인 시 자동 시작할 수 있다.
- UI는 영어이며 29개 theme family(44개 variant), 사용자 지정 primary color, UI·content·code 폰트 독립 선택을 지원한다.

## 자세히

구조는 휴대폰/브라우저가 HTTPS+WSS로 프록시(Tailscale Serve, Caddy, Nginx 등)에 접속하고, 프록시가 Mac의 aside-relay(:8799)로 전달하며, 이것이 Aside CLI·로컬 daemon·브라우저 프로필과 연결되는 형태다. 정적 파일(web/dist)만 호스팅해서는 대화, 에이전트 실행, 브라우저 도구, WebSocket, 푸시가 동작하지 않으며, 브리지와 Aside는 반드시 같은 Mac에서 함께 돌아야 한다.

인증은 loopback 요청이거나 검증된 Cloudflare Access JWT일 때만 자동 로그인되고, 그 외에는 토큰을 한 번 붙여넣어 쓴다. 공개 프록시에서는 `/api/web-token`을 막으라고 안내하며, 대화·브라우저 조작·업로드 API는 토큰이 필요하다. 정적 UI와 `/api/health`는 공개다. Mac이 깨어 있고 온라인이어야 하며 로그인 이전이나 로그아웃 후에는 브라우저 접근이 되지 않는다.

저장소에는 Python 백엔드(server.py, daemon.py, runner.py, web_push.py 등)와 `web/` 프론트엔드, 테스트(Playwright), DESIGN.md, 59개 커밋이 있다. 원문은 Tailscale 설정 절 중간에서 잘려 이후의 Caddy/Nginx, Cloudflare, iPhone 설치, 설정 레퍼런스, 문제 해결 부분은 읽지 못했다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 로컬에서 도는 에이전트의 승인 요청·후속 메시지 큐·steering·브라우저 제어를 원격 UI로 노출하는 설계 사례로 참고할 만하다. 또한 에이전트 접근 토큰 하나가 실행 권한과 로그인된 브라우저 접근을 함께 주는 보안 모델도 눈여겨볼 점이다.
