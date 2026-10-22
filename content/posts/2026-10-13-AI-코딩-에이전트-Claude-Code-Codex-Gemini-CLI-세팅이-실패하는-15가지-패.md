---
title: "AI 코딩 에이전트(Claude Code·Codex·Gemini CLI) 세팅이 실패하는 15가지 패턴과 해결법"
date: "2026-10-13"
description: "AI 코딩 에이전트(Claude Code·Codex·Gemini CLI)를 몇 주 쓰면 \"일은 많이 했는데 반영된 건 없는\" 상태가 되는데, 원인은 대개 모델 성능이 아니라 운영 설정이라는 글이다. 15가지 실패 패턴 중 7가지와 해결법이 요약돼 있다."
category: "AI 에이전트·모델"
tags: ["코딩-에이전트", "AGENTS.md", "PreToolUse-훅", "sandbox/approval", "멱등성", "에이전트-메모리"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35266"
original: "https://adrianstudio3.hashnode.dev/15-ways-ai-coding-agents-fail-in-real-repos-and-the-guardrails-that-fix-them"
coverage: "summary-only"
---

> 📌 2026-10-13 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35266) · [원문 기사](https://adrianstudio3.hashnode.dev/15-ways-ai-coding-agents-fail-in-real-repos-and-the-guardrails-that-fix-them)
> 근거: GeekNews 요약 페이지 — ⚠️ 원문 기사: HTTP Error 403: Forbidden

## 한 줄 요약

AI 코딩 에이전트(Claude Code·Codex·Gemini CLI)를 몇 주 쓰면 "일은 많이 했는데 반영된 건 없는" 상태가 되는데, 원인은 대개 모델 성능이 아니라 운영 설정이라는 글이다. 15가지 실패 패턴 중 7가지와 해결법이 요약돼 있다.

## 핵심

- 증거 없는 "완료": 완료 기준에 "실행한 명령과 실제 결과"(예: "make test → 42 passed in 6.1s")를 넣게 한다. 고치는 시도는 3번까지만 하고 넘으면 멈추고 보고하게 하면, 실패하는 테스트를 지워버리는 일이 사라진다고 한다.
- 도구마다 다른 지침: 규칙은 AGENTS.md 하나에 두고, CLAUDE.md는 @AGENTS.md 로 불러오며, Gemini는 context.fileName 으로 읽게 한다. 지침이 서로 달라지면 CI에서 걸리게 해둔다.
- sandbox와 approval 혼동: sandbox는 접근 가능한 경계이고, approval은 경계를 넘으려 할 때의 처리 방식이다. Codex의 approval_policy = "never" 는 권한 확대가 아니라 경계를 넘는 작업을 묻지 않고 실패시키는 설정이라, 새벽 배치가 아무것도 안 하는 원인이 되곤 한다. untrusted 는 이제 지원되지 않는다.
- 프롬프트 속 "하지 마" 규칙은 PreToolUse 훅으로 옮긴다. exit 2로 끝나면 실행이 막히고 이유가 에이전트에게 전달되며, 정규식 20줄 정도로 rm -rf, 강제 push, curl | sh, .env 읽기를 막을 수 있다.
- 리뷰어 서브에이전트에는 Edit 권한을 주지 않거나, 다른 CLI를 read-only 프로필로 돌려 교차 리뷰를 한다.
- 기억은 세 층(턴 로그, 하루 요약 40줄 이하, 장기 기억 120줄 이하)으로 나누고, 매주 반복해서 나온 것만 장기 기억으로 올리며 오래된 건 지운다.
- 타임아웃 뒤 중복 실행을 막으려면 외부로 나가는 작업에 멱등성 키를 붙이고, 응답이 없으면 unknown 상태로 두며 재전송 전에 상대 시스템에서 실제 처리 여부를 확인한다.

## 왜 골랐나

AI 에이전트 개발 관심사와 직접 맞닿는다. 완료 증거 요구, 시도 횟수 제한, 훅 기반 가드레일, 읽기 전용 리뷰어, 계층형 메모리, 멱등성 키는 에이전트 운영 설계에 바로 참고할 수 있는 항목이다.
