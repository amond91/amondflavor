---
title: "Your AI agent isn't dumber, it's full"
date: "2026-11-04"
description: "AI 에이전트가 한 시간쯤 지나 엉뚱해지는 것은 성능 저하가 아니라 컨텍스트가 가득 찼기 때문이며, 해법으로 planner·implementer·reviewer 3-에이전트 오케스트레이션(Claude Code subagent + /build-feature 스킬)을 제시합니다. 나머지 두 가지 해법(skills, CLI 우선)과 Vue School Black Friday 할인 홍보가 함께 실린 뉴스레터입니다."
category: "AI 에이전트·모델"
tags: ["컨텍스트-윈도우", "에이전트-오케스트레이션", "Claude-Code-subagent", "planner-implementer-reviewer", "MCP-vs-CLI", "Agent-Skills"]
source: "뉴스레터"
coverage: "full"
---

> 📌 2026-11-04 · 뉴스레터
> 근거: 메일 본문 + 링크 기사

## 한 줄 요약

AI 에이전트가 한 시간쯤 지나 엉뚱해지는 것은 성능 저하가 아니라 컨텍스트가 가득 찼기 때문이며, 해법으로 planner·implementer·reviewer 3-에이전트 오케스트레이션(Claude Code subagent + /build-feature 스킬)을 제시합니다. 나머지 두 가지 해법(skills, CLI 우선)과 Vue School Black Friday 할인 홍보가 함께 실린 뉴스레터입니다.

## 핵심

- 증상: 처음 20분은 좋지만 한 시간쯤 지나면 이미 있는 composable을 다시 만들고, Pinia store를 무시하며, 합의한 API를 잊습니다. 저자는 읽고 검색하고 실행한 모든 것이 컨텍스트에 남아 중요한 결정이 파일 30개 밑에 묻히기 때문이라고 진단합니다.
- Fix 1 (오케스트레이션): 한 에이전트가 계획을 세우고 조각을 다른 에이전트에 넘기며, 각 에이전트는 새 컨텍스트로 시작해 한 가지 일만 하고 짧은 요약을 돌려줍니다. planner는 코드를 읽고 계획만 쓰며 코드를 건드리지 않고, implementer는 계획을 따라 구현하고 테스트를 통과시키며, reviewer는 diff를 계획과 대조하되 수정은 금지됩니다.
- 구현 방식(링크된 unlearn.dev 글): 각 에이전트는 .claude/agents/ 의 Markdown 파일이고 frontmatter에서 tools를 명시합니다(planner: Read/Grep/Glob/Write, implementer: Edit·Bash 포함, reviewer: Edit·Write 없이 Bash). 계획은 plans/<feature>.md 파일로 공유해 main session 요약 과정의 정보 손실을 피하고, 에이전트에는 Agent 도구를 주지 않아 위임은 main session에서만 일어납니다.
- /build-feature 스킬(.claude/skills/build-feature/SKILL.md)이 전체를 실행합니다. 계획 작성 후 사용자 승인을 기다리는 체크포인트가 있고, reviewer가 CHANGES NEEDED 판정을 내리면 implementer로 되돌리되 최대 2라운드까지만 반복합니다. 계획을 고치는 데는 1분, 잘못된 계획 위에 만든 코드를 고치는 데는 오후 한나절이 든다고 합니다.
- 한계: 오타·이름 변경, 원인을 이미 아는 버그, 탐색적 작업에는 에이전트 하나가 더 빠르고 쌉니다. 에이전트마다 코드를 다시 읽어 비용이 늘기 때문입니다. 또 reviewer에게 Bash가 있어 파일 변경을 막는 것은 도구 제한이 아니라 프롬프트 지시일 뿐이라고 원문이 인정합니다.
- Fix 2: Vue/Nuxt 규칙을 skills로 한 번만 써 두면 필요할 때만 불러옵니다(Vue School 강좌 'AI Agent Skills for Vue Developers' 소개). Fix 3: MCP 서버보다 CLI를 먼저 시도하라는 것으로, Claude Code에서 같은 작업을 돌렸더니 답은 같았지만 MCP가 토큰을 2.1배 썼고, PR 조회 한 건이 CLI 135자 대 MCP 19,564자였다고 합니다.
- 후반부는 Vue School/Mastering Nuxt Black Friday 홍보입니다. Super Early Bird $149(첫 해, 이후 연 $299 갱신), 개별 구매 시 $1,417이라고 하며, 공식 Vue.js 인증 시험 무료 혜택은 11월 10일 22:00 CET까지입니다.

## 자세히

이 글의 핵심 논지는 컨텍스트 격리입니다. subagent는 자체 컨텍스트 창에서 시작해 자신의 지시문, main session이 쓴 작업 메시지, CLAUDE.md, git status 스냅샷만 받고 대화 내용은 보지 못합니다. 작업이 끝나면 최종 메시지만 main session에 돌아오므로 planner가 파일 30개를 읽어도 그 내용은 main session이나 implementer의 컨텍스트에 남지 않습니다. 반대로 subagent는 지시문·작업 메시지·읽을 수 있는 파일만 알기 때문에, 이 구조에서 실제 설계 작업은 에이전트 사이에 무엇을 넘길지 정하는 일이라고 합니다.

독립 검증도 설계 의도입니다. reviewer는 새 컨텍스트로 시작하므로 implementer의 추론이나 가정을 물려받지 않고, 계획·diff·테스트만 가지고 판단합니다. reviewer 프롬프트에는 '문제를 지어내지 말라'는 문장이 있는데, 문제를 찾으라고 지시받은 reviewer는 코드에 문제가 없어도 몇 개를 보고하는 경향이 있기 때문이라고 설명합니다. 확장 아이디어로는 reviewer를 보안·성능·테스트 전문 리뷰어로 나눠 병렬 실행하기, frontmatter의 model 필드로 implementer에 저렴한 모델 쓰기, 계획에서 실패하는 테스트를 먼저 쓰는 test writer 추가하기가 제시됩니다.

원문은 뉴스레터 본문과 링크된 unlearn.dev 글, Vue School 강좌 페이지로 구성됩니다. 뉴스레터의 Fix 3 수치(2.1배, 135자 대 19,564자)는 링크된 비교 글(unlearn.dev/comparisons/mcp-vs-cli)에서 나온 것이며 그 글은 수집되지 않았습니다. 강좌 페이지는 12강·1시간 분량의 목차만 담고 있습니다.

## 왜 골랐나

AI 에이전트 개발 관심사와 직접 맞닿습니다. 컨텍스트 격리, 파일 기반 핸드오프, 승인 체크포인트, 라운드 상한 같은 멀티 에이전트 설계 패턴과, 도구 제한(강제)과 프롬프트 지시(비강제)의 차이를 구체적인 예로 볼 수 있습니다. 다만 수치와 주장은 저자 회사(Vue School 등)의 자체 보고이며 판매 목적의 뉴스레터에 실린 것입니다.
