---
title: "JetBrains Thinkrail - 명세로 프로젝트 지식을 유지하는 에이전트 IDE"
date: "2026-10-26"
description: "JetBrains 의 ThinkRail 은 pi 코딩 에이전트를 감싼 에이전트 IDE로, git worktree 기반 작업공간 격리와 코드 옆 SPEC.md 그래프(명세)·재사용 가능한 스킬로 세션을 넘어 프로젝트 지식을 유지하는 것이 핵심이다. 데스크톱 앱과 브라우저용 CLI로 제공되며 Apache 2.0 라이선스다."
category: "AI 에이전트·모델"
tags: ["ThinkRail", "JetBrains", "pi-코딩-에이전트", "SPEC.md", "git-worktree", "Agent-Skills"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35994"
original: "https://github.com/JetBrains/thinkrail"
coverage: "full"
---

> 📌 2026-10-26 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35994) · [원문 기사](https://github.com/JetBrains/thinkrail)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

JetBrains 의 ThinkRail 은 pi 코딩 에이전트를 감싼 에이전트 IDE로, git worktree 기반 작업공간 격리와 코드 옆 SPEC.md 그래프(명세)·재사용 가능한 스킬로 세션을 넘어 프로젝트 지식을 유지하는 것이 핵심이다. 데스크톱 앱과 브라우저용 CLI로 제공되며 Apache 2.0 라이선스다.

## 핵심

- git 저장소를 프로젝트로 열고 git worktree 로 작업공간을 만들면 각각 독립된 브랜치와 작업 디렉터리를 가져, 에이전트가 기존 체크아웃을 건드리지 않고 병렬로 일한다. PR 병합·종료 시 또는 일주일간 활동이 없으면 작업공간이 프로젝트별 보관 선반으로 자동 이동하고, 다시 작업하면 복귀한다.
- 상단 표시줄에서 origin 대비 상태와 PR 상태를 보고 게시·푸시·PR 생성을 실행한다. GitHub PR을 연 뒤에는 CI 검사와 리뷰 결정을 확인하고, 실패한 검사나 분기된 브랜치 해결을 에이전트에 맡기거나 병합·재오픈·원격 브랜치 삭제를 할 수 있다.
- 모든 모듈 옆에 연결된 SPEC.md 파일 그래프를 두고 에이전트가 전용 spec_* 도구로 읽고 검색하고 관리한다. 다른 코딩 에이전트용 Agent Skills 도 원래 위치에서 그대로 읽으며, 프로젝트 설정·설계 브레인스토밍·PR 제출 워크플로가 기본 제공된다.
- 작업공간마다 여러 동시 pi 세션을 실행하며 세션별로 모델과 실시간 토큰·비용을 표시한다. 실행 중 방향 지시, 후속 요청 대기열, 하위 에이전트 위임, 사용자가 편집 가능한 공동 계획(단계별 커밋·검토)을 지원하고, 채팅 안에서 다이어그램·선택지 비교·UI 전후 목업도 렌더링한다.
- pi 의 mcp.json 에 있는 MCP 서버를 연결할 수 있다. 읽기 전용으로 표시되지 않은 호출은 채팅에서 승인받고, 저장소가 정의한 서버는 사용자가 승인하기 전까지 비활성이다. 인증은 JetBrains AI 로그인 또는 pi 자체 인증을 쓰며 ThinkRail 자체 계정은 없다.
- 기본 사용 이벤트(실행·채팅 생성·메시지 전송·제공업체 연결)는 무작위 설치 ID와 연결해 수집하고, 오류 보고는 항상 전송된다. 프롬프트·코드·파일·인증 정보는 포함하지 않으며, 추가 사용 데이터 공유만 Settings → Privacy 또는 --no-analytics / THINKRAIL_NO_ANALYTICS=1 로 끌 수 있다.

## 자세히

구조상 ThinkRail 은 pi 코딩 에이전트를 프로세스 내부에서 실행해 웹 UI에 연결하는 얇은 호스트다. pi 가 모델·스킬·맥락 압축·비용·세션 상태를 담당하고, 앱은 작업공간·편집기·통신을 맡으며 사용자 모르게 프롬프트를 조립하지 않는다고 한다. 구성은 packages/server·shared(Bun.serve 기반 HTTP+WebSocket 엔진 호스트, 채팅당 프로세스 내부 pi 세션 하나), packages/contracts(타입·버전이 있는 프로토콜), apps/web(React 19, Zustand, Tailwind v4 클라이언트)의 세 영역이다.

개발 방식도 명세 우선이다. 모든 모듈 옆에 SPEC.md 를 두고, 경계·계약·결정이 바뀌면 같은 변경에서 명세를 갱신한다. 공식 설계 문서는 goal-and-requirements.md 와 architecture.md 이다. 개발 환경은 Bun 1.4.4 와 Node.js 22.19 이상이 필요하다.

설치는 macOS Apple Silicon·Windows x64·Linux x64/ARM64 데스크톱 앱과, install.sh / install.ps1 로 설치하는 thinkrail CLI 두 가지다. macOS Intel 용 데스크톱 빌드는 없다. Linux 아티팩트는 서명되지 않았고 macOS CLI는 서명되었으나 아직 notarize 되지 않았다. 앞으로 전체 확장 지원을 추가해 ThinkRail 이 사용자의 작업 방식에 맞는 도구를 스스로 만들게 할 계획이다. GitHub 저장소는 1.8k stars, 105 forks, 554 commits 로 표시된다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 명세(SPEC.md 그래프)로 세션 간 맥락을 유지하고 worktree 로 에이전트를 병렬 격리하는 설계, 기존 Agent Skills·MCP 를 그대로 재사용하는 구조가 참고할 만하다.
