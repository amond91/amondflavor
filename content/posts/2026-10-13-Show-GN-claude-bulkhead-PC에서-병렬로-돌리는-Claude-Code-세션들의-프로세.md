---
title: "Show GN: claude-bulkhead: PC에서 병렬로 돌리는 Claude Code 세션들의 프로세스, 포트, 메모리 충돌을 막는 플..."
date: "2026-10-13"
description: "claude-bulkhead는 한 PC에서 여러 Claude Code 세션(주로 git worktree별)을 병렬로 돌릴 때 생기는 프로세스 종료·포트·메모리·git 트리 충돌을 PreToolUse hook으로 사전 차단하는 Claude Code 플러그인입니다. 위험한 명령은 막고 Claude에게 대신 실행할 안전한 명령을 안내합니다."
category: "AI 에이전트·모델"
tags: ["Claude-Code", "hooks", "git-worktree", "병렬-에이전트", "프로세스·포트-충돌", "메모리-가드"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35290"
original: "https://github.com/Myeongseok-Kang/claude-bulkhead"
coverage: "partial"
---

> 📌 2026-10-13 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35290) · [원문 기사](https://github.com/Myeongseok-Kang/claude-bulkhead)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

claude-bulkhead는 한 PC에서 여러 Claude Code 세션(주로 git worktree별)을 병렬로 돌릴 때 생기는 프로세스 종료·포트·메모리·git 트리 충돌을 PreToolUse hook으로 사전 차단하는 Claude Code 플러그인입니다. 위험한 명령은 막고 Claude에게 대신 실행할 안전한 명령을 안내합니다.

## 핵심

- 문제 상황: 한 에이전트가 `taskkill /F /IM node.exe`나 `pkill node`를 실행하면 다른 worktree 세션의 개발 서버·테스트가 함께 죽고, 세 세션이 동시에 빌드하면 메모리 부족으로 PC가 멈춰 강제 재부팅까지 발생했다고 합니다.
- 가드 7종: kill-guard(프로그램 이름 기반 종료 차단, PID 기반 종료는 허용), port-guard(다른 세션이 쓰는 포트로 서버 시작 차단 후 빈 포트 안내), memory-guard/bulkhead-slot(여유 RAM이 3GB 미만이거나 20GB 미만 머신에서 15% 미만이면 무거운 명령 차단, 공유 RAM 예산 큐), checkout-guard(같은 트리를 쓰는 다른 세션이 있으면 checkout·reset --hard 등 차단 후 git worktree add 제안), wait-guard(180초 넘는 포그라운드 sleep 차단), orphan-reaper(세션 종료 후 남은 개발 서버·테스트 워커 등을 몇 초 안에 정리), basics(rm -rf 루트/홈, main 강제 푸시, 미커밋 변경 손실 reset 차단).
- 동작 방식: 각 가드는 의존성 없는 독립 Node 스크립트이며, Claude Code가 JSON으로 넘긴 명령을 검사해 해당할 때만 프로세스 테이블, 리스닝 포트, ~/.claude/sessions, 여유 RAM, git status를 확인합니다. 차단은 exit code 2와 대체 명령 메시지이고, 가드 내부 오류가 나면 통과(exit 0)시켜 작업을 막지 않습니다.
- 관측·설정: `bulkhead status/top/log/doctor/port/reap` 명령, 상태표시줄(예: `3 sessions 2 heavy 9.1 GB free`), 슬래시 명령 5개를 제공합니다. 전역(~/.claude/bulkhead/config.json)·프로젝트(<repo>/.claude/bulkhead.json) 설정과 `BULKHEAD_DISABLE` 환경변수로 가드별 끄기·허용 목록 지정이 가능합니다.
- 선택 기능 `bulkhead remote`: 무거운 작업을 SSH로 다른 PC에서 실행합니다. 현재 커밋(git bundle)과 미커밋 변경만 전송하고, 원격에 최대 8개 저장소 사본을 두며, 종료나 SSH 연결 끊김 시 원격에서 띄운 프로세스를 모두 정리합니다. 원격에는 SSH 서버, bash, git, tar만 필요합니다.
- 설치는 `/plugin marketplace add Myeongseok-Kang/claude-bulkhead` 후 `/plugin install bulkhead@bulkhead`이며 Node 18 이상이 PATH에 있어야 합니다. Node가 없으면 모든 hook이 실패해 아무것도 차단되지 않습니다.
- README는 Claude Code 이슈 트래커의 사례(#13941, #34754, #43225, #53229, #15487, #11502, #25180)를 근거로 듭니다. 작성자는 잘못 차단되는 명령이 있으면 이슈로 알려 달라고 요청했습니다.

## 자세히

이 프로젝트는 일반적인 위험 명령 차단 hook 모음(claude-forge, dcg, cc-safety-net 등)과 목적이 다르다고 README가 구분합니다. bulkhead는 한 머신 위 세션들이 프로세스 소유권, 포트, 공유 RAM, 공유 git 트리를 두고 서로 방해하는 문제에 초점을 맞춥니다. basics 3개 규칙만 다른 팩과 겹치며 `BULKHEAD_DISABLE=basics`로 끌 수 있습니다.

모든 세션은 하나의 상태 디렉터리(~/.claude/bulkhead)를 공유합니다. bulkhead-slot의 RAM 예약과 차단 기록(guards.log, events.jsonl)이 여기에 저장되고, 이벤트 로그는 1MB에서 회전하며 로컬에만 남습니다. 세션은 ~/.claude/sessions 파일로 식별합니다. Windows, Linux, macOS를 지원하며, macOS는 fixture로만 테스트했다고 명시되어 있습니다.

원문은 마지막의 상태표시줄 설명 부분에서 잘려 있어, 그 뒤의 FAQ 등은 확인하지 못했습니다.

## 왜 골랐나

AI 에이전트 개발 관심사와 닿습니다. 여러 코딩 에이전트와 서브에이전트를 병렬 운용할 때 공유 자원(프로세스·포트·RAM·git 트리) 충돌을 PreToolUse hook으로 막는 설계 패턴을 볼 수 있습니다.
