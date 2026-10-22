---
title: "tmux와 Claude Code: 세션, send-keys, 그리고 한계"
date: "2026-10-20"
description: "tmux는 AI 코딩 에이전트를 터미널 종료·SSH 단절에도 살아남는 이름 있는 세션으로 운영하게 해 주고, send-keys/capture-pane으로 외부 스크립트 제어도 가능하다. 그러나 에이전트의 승인 대기·완료 같은 상태, 구조화된 기록, 세션 간 공유, 재부팅 후 보존은 해결하지 못하며, 글은 이 공백을 메우는 macOS 터미널 crystl을 제안한다."
category: "AI 에이전트·모델"
tags: ["tmux", "Claude-Code", "AI-코딩-에이전트", "send-keys", "git-worktree", "crystl"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35650"
original: "https://crystl.dev/guides/tmux-for-ai-agents/"
coverage: "full"
---

> 📌 2026-10-20 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35650) · [원문 기사](https://crystl.dev/guides/tmux-for-ai-agents/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

tmux는 AI 코딩 에이전트를 터미널 종료·SSH 단절에도 살아남는 이름 있는 세션으로 운영하게 해 주고, send-keys/capture-pane으로 외부 스크립트 제어도 가능하다. 그러나 에이전트의 승인 대기·완료 같은 상태, 구조화된 기록, 세션 간 공유, 재부팅 후 보존은 해결하지 못하며, 글은 이 공백을 메우는 macOS 터미널 crystl을 제안한다.

## 핵심

- tmux 서버가 pty를 소유하므로 터미널 창을 닫거나 SSH 연결이 끊겨도 에이전트가 계속 실행된다. 여러 에이전트를 이름 있는 세션으로 두고 키 입력으로 전환할 수 있으며, 별도 IPC나 API 없이 외부 셸에서 제어할 수 있다.
- Claude Code의 `claude --worktree [name] --tmux`는 Git 작업 트리와 tmux 세션을 함께 만든다. `--tmux`는 `--worktree` 없이 쓰면 오류가 나고, iTerm2에서는 기본이 네이티브 패널이라 스크립트로 제어하려면 `--tmux=classic`을 지정해야 한다.
- send-keys로 프롬프트를 보낼 때는 `-l`(리터럴)로 텍스트를 보내고, 에이전트 입력창의 디바운스·재렌더링 때문에 `sleep 0.3` 뒤 Enter를 별도 호출로 보내는 패턴을 권장한다. 끝의 세미콜론은 `\;`로 이스케이프해야 한다. 화면 읽기는 `capture-pane -p -J`(줄바꿈 이어 붙이기), 지속 기록은 `pipe-pane`을 쓴다.
- tmux는 에이전트 상태를 알지 못한다. 상태 표시줄 스크립트로 "Do you want"·"esc to interrupt" 같은 화면 문자열을 5초마다 검사하거나 `monitor-silence 30`으로 출력 중단을 감지할 수 있지만, 둘 다 실제 상태가 아니라 TUI 렌더링이나 완료의 대리 신호에 의존해 취약하다.
- 창 닫기·detach·SSH 단절에는 에이전트가 유지되지만 `kill-session`, `kill-server`, 재부팅에서는 사라지고 스크롤백도 함께 없어진다. 기록이 중요하면 `pipe-pane`으로 파일에 저장해야 한다.
- 원격 머신·서버 장기 프로세스·불안정한 SSH에는 `ssh devbox -t tmux new-session -A -s agents`처럼 tmux를 계속 쓰길 권한다. 중첩 tmux의 접두 키 충돌은 원격 접두 키 변경이나 `send-prefix`로 푼다.
- crystl(macOS 터미널)은 승인 요청 플로팅 카드, 에이전트 상태 패널, 턴·도구 호출·결과의 타입 있는 레코드 저장, 재시작 후에도 남는 기록, 실제 Git 작업 트리 기반 격리 세션을 내세운다. 글의 출처가 crystl 자사 가이드이므로 홍보 성격이 있다.

## 자세히

글은 먼저 tmux가 에이전트 실행에 맞는 이유(연결 유지, 다중 세션, 외부 스크립트 제어)를 설명한 뒤, 프로젝트별 세션·에이전트별 창 구성과 에이전트용 `.tmux.conf` 설정(`history-limit 100000`, `allow-rename off`, `escape-time 10`, `monitor-activity` 등)을 안내한다. 패널 분할 시 `-c '#{pane_current_path}'`를 빼면 엉뚱한 저장소에서 에이전트가 실행될 수 있고, `main-pane-width`의 백분율 지정은 tmux 3.2 이상이 필요하다는 실무 주의점도 있다.

한계 부분의 논지는 tmux가 "한 사람이 키보드로 여러 셸을 쓰는" 다중화 도구라는 범위에서 비롯된다는 것이다. 승인 대기·생각 중·완료가 모두 텍스트 패널로만 보이고, capture-pane은 구조화된 발화·도구 호출 결과가 아니라 스피너와 테두리 문자가 섞인 렌더링 화면만 주므로 UI가 바뀌면 파서가 깨진다. 세션 간 공유 상태가 없어 인계는 파일·잠금·폴링으로 직접 만들어야 하고, 12개 패널에 각 10만 줄 스크롤백은 메모리 비용이 크다고 지적한다.

결론은 tmux를 버리라는 것이 아니라, 원격·서버·SSH 중심에는 tmux, 여러 에이전트가 예측 불가한 시점에 결정을 요구하는 상황에는 에이전트 인지형 도구가 맞는다는 구분이다. 다만 crystl의 기능 설명은 원문 자체 주장이며 독립 검증은 없다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 화면 스크래핑 대신 구조화된 이벤트·기록으로 에이전트 상태를 다뤄야 한다는 논점과 `--worktree` 기반 병렬 에이전트 격리·tmux 제어 패턴이 직접 참고가 된다.
