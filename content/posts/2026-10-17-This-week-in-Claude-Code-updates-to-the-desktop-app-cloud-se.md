---
title: "This week in Claude Code: updates to the desktop app, cloud sessions, and more"
date: "2026-10-17"
description: "Claude Code 데스크톱 앱에 새 세션 화면·중첩 세션(nested sessions)·사이드 레일이 추가됐고, 클라우드 세션은 커밋/푸시하지 않은 로컬 상태도 그대로 이어받게 됐습니다. Max·Team 플랜에는 월간 API 크레딧이 제공되고 Haiku 5.5 등 모델·요금 소식도 함께 전해졌습니다."
category: "AI 에이전트·모델"
tags: ["Claude-Code", "중첩-세션", "클라우드-세션", "서브에이전트", "Managed-Agents", "API-크레딧"]
source: "뉴스레터"
coverage: "partial"
---

> 📌 2026-10-17 · 뉴스레터
> 근거: 메일 본문 + 링크 기사

## 한 줄 요약

Claude Code 데스크톱 앱에 새 세션 화면·중첩 세션(nested sessions)·사이드 레일이 추가됐고, 클라우드 세션은 커밋/푸시하지 않은 로컬 상태도 그대로 이어받게 됐습니다. Max·Team 플랜에는 월간 API 크레딧이 제공되고 Haiku 5.5 등 모델·요금 소식도 함께 전해졌습니다.

## 핵심

- 데스크톱 앱 새 세션 화면: 최근 폴더를 바로 열 수 있고, 여러 폴더·SSH 호스트·클라우드 설정을 '환경(environment)'으로 저장해 그곳에서 세션을 시작할 수 있습니다.
- 중첩 세션: Claude에게 작업을 병렬로 시키면 각각 자체 중첩 세션을 갖고, 끝나면 메인 세션에 결과를 보고합니다. 후속 작업은 클릭 한 번으로 별도 세션에서 시작하며, /btw 는 사이드 질문을 별도 탭에서 엽니다. 사이드바는 어떤 세션이 작업 중이고 어떤 세션이 사용자 입력을 기다리는지 보여주고, 새 사이드 레일은 세션의 실행 위치·컨텍스트·변경 내용을 보여줍니다.
- 클라우드 세션: 로컬 세션을 클라우드로 이어갈 때 커밋하지 않은 변경과 푸시하지 않은 커밋까지 그대로 가져갑니다. Team 플랜에서도 클라우드 환경의 네트워크 시크릿을 쓸 수 있어, API 키와 대상 호스트를 등록하면 Claude가 키를 보지 않고 해당 API를 호출합니다.
- 요금·모델: Max 5x $100, Max 20x $200, Team 풀 최대 $500의 월간 API 크레딧이 제공됩니다. Haiku 5.5는 1M 컨텍스트 윈도우를 갖고(/model haiku), Sonnet 5.5 캐시 읽기는 100만 토큰당 $0.20에서 $0.10으로 내렸으며, Opus 5.5 Fast mode는 Claude API에서 정식 출시(최대 2.5배 출력 속도, "speed": "fast")됐습니다.
- 서브에이전트별 effort: 프롬프트에서 서브에이전트마다 effort 수준을 지정해 세션 설정을 덮어쓸 수 있습니다(v2.1.292 이전에는 커스텀 서브에이전트 frontmatter에 써야 했음). 모델도 같은 방식으로 지정할 수 있습니다.
- Managed Agents(beta): Anthropic 인프라에서 에이전트를 돌리며, `/claude-api managed-agents-onboard <블로그 URL>` 로 Slack·GitHub를 일정에 따라 읽고 일일 브리프를 올리는 에이전트를 자동 설정할 수 있습니다.
- 기타 changelog: 2.1.299 유휴 세션 자동 compaction(idleCompaction 설정), /usage 경고 추가, UserPromptSubmit 훅이 스킬·슬래시 명령 확장 전에 실행; 2.1.295 훅 "onFailure": "block"; 2.1.292 plugin install --marketplace; 2.1.290 웹 검색 한도 시간당 100회 리필. OSS Scanner 무료 보안 스캔 신청도 안내됐습니다.

## 자세히

뉴스레터 본문은 최근 2주간의 데스크톱 앱 업데이트를 중심으로 구성돼 있습니다. 여러 세션을 동시에 돌리는 사용 방식을 전제로 세션 시작(환경 저장)·병렬 실행(중첩 세션)·상태 파악(사이드바, 사이드 레일)을 모두 손본 것이 핵심입니다.

링크된 문서에 따르면 클라우드 세션은 Anthropic이 관리하는 인프라(또는 조직의 자체 호스팅 환경)에서 돌아가 노트북을 닫아도 계속되며, 플랜 사용 한도에 합산되고 클라우드 VM 비용은 별도로 없습니다. 터미널에서는 `claude --cloud` 로 시작하고 `--teleport` 로 클라우드 세션을 로컬로 가져옵니다. 문서에는 CLI에서 기존 터미널 세션을 클라우드로 보낼 수 없고, 데스크톱 앱은 Open in 메뉴로 보낼 수 있다고 적혀 있습니다. Zero Data Retention·HIPAA 구성 조직과 CMEK 사용 조직은 클라우드 세션을 쓸 수 없다고 합니다.

엔지니어링 팀 팁으로, 서브에이전트에 "Time matters here: do not spend time that can be avoided, and the earlier a correct result is obtained, the better." 한 문장을 프롬프트에 넣으면 속도와 품질 균형이 적절해진다고 소개됩니다. 두 번째 링크 기사(클라우드 세션 문서)는 중간에서 잘려 있습니다.

## 왜 골랐나

AI 에이전트 개발 관점에서 서브에이전트별 effort·모델 지정, 중첩 세션을 통한 병렬 에이전트 오케스트레이션, Managed Agents(beta)가 직접 관련됩니다.
