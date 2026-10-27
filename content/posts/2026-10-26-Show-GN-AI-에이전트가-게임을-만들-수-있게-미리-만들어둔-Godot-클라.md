---
title: "Show GN: AI 에이전트가 게임을 만들 수 있게 미리 만들어둔 Godot 클라이언트 코어"
date: "2026-10-26"
description: "open-game-works는 반복적으로 필요한 클라이언트·서버 기능(UI, 네트워크, 인증, 결제, 튜토리얼 등)을 미리 만든 core(Godot 클라이언트 43개 모듈, TypeScript 서버 42개 모듈)로 분리해 두고, AI 에이전트는 게임플레이 코드만 쓰게 하는 하네스입니다. `create game: <게임이름>` 명령 하나로 에이전트가 코드 작성, 테스트, 직접 플레이, 판정까지 반복합니다."
category: "AI 에이전트·모델"
tags: ["AI-에이전트", "Godot", "Claude-Code", "게임-개발-자동화", "core/content-분리", "에셋-생성-파이프라인"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36020"
original: "https://github.com/wnsrn3436/open-game-works-core"
coverage: "partial"
---

> 📌 2026-10-26 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36020) · [원문 기사](https://github.com/wnsrn3436/open-game-works-core)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

open-game-works는 반복적으로 필요한 클라이언트·서버 기능(UI, 네트워크, 인증, 결제, 튜토리얼 등)을 미리 만든 core(Godot 클라이언트 43개 모듈, TypeScript 서버 42개 모듈)로 분리해 두고, AI 에이전트는 게임플레이 코드만 쓰게 하는 하네스입니다. `create game: <게임이름>` 명령 하나로 에이전트가 코드 작성, 테스트, 직접 플레이, 판정까지 반복합니다.

## 핵심

- 작성자는 Claude Code로 게임을 만들며, 에이전트가 게임 로직은 잘 짜지만 UI·네트워크·로그인·입력 처리·튜토리얼은 매번 새로 짜고 어딘가 깨져 있었다고 느껴 이 부분을 core로 분리했습니다.
- 클라이언트 core는 Godot 기반 43개 모듈(UICore, NetworkCore, AuthCore, PaymentCore, RealtimeCore, TutorialCore, QACore 등)이고, 서버 core는 TypeScript 42개 모듈입니다. 모듈마다 스펙 문서가 있어 같은 스펙으로 다른 엔진(Three.js, Phaser, Unity, Unreal)용 core도 만들 수 있게 해 두었습니다.
- 게임은 core를 직접 수정하지 않고 데이터와 훅으로만 연결합니다. core가 부족하면 content agent는 core를 고치는 대신 우회 구현을 하고 `records/core-proposals/`에 개선 제안을 남기며, 사람이 `update core`로 적용할 제안을 고릅니다.
- 루프는 content agent가 코드를 수정하고, 자동 검사(regression test, static check, no blocker, performance, no placeholder, full flow, UI quality, row simulation)를 돌린 뒤, play agent가 실제 입력으로 플레이하고, judge agent가 새 컨텍스트에서 baseline 게임과 A/B 블라인드 비교 및 레퍼런스 게임 플레이 영상과 비교하는 순서로 돕니다. 중단 사유는 success, repeated defect, oscillation, plateau, limit reached, stopped by human입니다.
- QACore가 에이전트가 실제 입력으로 게임을 플레이해볼 수 있는 자동화 채널을 제공합니다. README의 예시 게임은 iso-survivors(아이소메트릭 3D 서바이벌), mana-clash(카드 배틀), pong, block-stack입니다.
- 리소스는 처음에 플레이스홀더로 개발하고 나중에 생성 방법으로 교체합니다. 이미지는 FLUX.2 klein, 음악은 ACE-Step, 효과음은 MOSS, 음성은 Qwen3-TTS, 3D는 TRELLIS.2나 Blender를 씁니다. 작성자는 이 리소스 제작이 품질 면에서 가장 갈 길이 멀다고 보고 에셋 생성 파이프라인 경험자의 의견을 구하고 있습니다.
- 현재 상태는 macOS(Apple silicon)에서 Claude Code(Claude Opus 5.5, high effort)로만 테스트했고, play·translation 하위 에이전트는 Claude Sonnet 5.5(medium effort)입니다. Codex CLI는 지원하나 미테스트이며, build 명령은 아직 실행해 보지 않아 이 도구로 빌드·배포된 게임은 없습니다.

## 자세히

구조는 harness(open-game-works-core: 에이전트 규칙, 스킬, 모듈 스펙, CLI `ogw`)와 core 구현(open-game-works-core-impl: Godot 클라이언트 core, TypeScript 서버 core, base proto, contract tool, data tool)으로 나뉩니다. 게임은 settings.json의 `coreVersion`으로 core 릴리스 버전만 고정하며, 한 버전이 client core, server core, contract tool, data tool, core settings를 함께 포함합니다.

역할은 9개 sub-agent(content, resource, translation, play, judge, server hardening, client hardening, core, review)로 나뉘고, 판단이 필요 없는 일(설정·기록 읽고 쓰기, 계산, 비교, 인스턴스 시작, 상태 전이)은 에이전트가 아니라 `ogw`가 맡습니다. 증거 순위는 자동 검사, telemetry, LLM 의견 순이며, 검증하지 못한 것은 실패가 아니라 unknown으로 기록합니다. play·judge 에이전트는 brief.md와 design.md를 보지 못하고, judge는 play 피드백도 받지 않습니다.

`build: <game> <DEV, STG, PROD>`는 hardening 후 android, ios, web, windows, macos, linux 빌드를 만들고, DEV·STG는 서버도 배포하며 PROD 배포는 사람이 합니다. 환경은 LOCAL(SQLite, fake adapter), DEV·STG·PROD(PostgreSQL)로 나뉩니다. 리소스 생성 도구는 출처를 확인할 수 없는 리소스를 쓰지 않고, 레퍼런스 게임의 화면·리소스·이름·텍스트를 생성 입력으로 쓰지 않는다고 명시합니다. 원문은 마지막 Settings 절(Secrets 부분)에서 잘려 있습니다.

## 왜 골랐나

AI 에이전트 개발 관심사와 직접 맞닿습니다. 에이전트가 못하는 영역을 core로 고정하고 변경 대신 제안만 남기게 하는 구조, 판단이 필요 없는 일을 CLI로 분리하는 원칙, play·judge 에이전트의 컨텍스트 격리와 증거 순위 설계는 에이전트 하네스 설계 사례로 참고할 만합니다.
