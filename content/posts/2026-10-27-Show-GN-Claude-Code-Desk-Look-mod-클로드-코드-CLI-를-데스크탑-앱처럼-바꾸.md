---
title: "Show GN: Claude Code Desk Look (mod) - 클로드 코드 CLI 를 데스크탑 앱처럼 바꾸기"
date: "2026-10-27"
description: "desk-look은 Claude Code CLI 터미널 화면을 Claude 데스크톱 앱의 Code 탭처럼 다시 그려 주는 MIT 라이선스 Claude Code 플러그인이다. 엔진은 건드리지 않고 화면 렌더링만 바꾸며, 작성자는 리눅스 터미널 환경에서 쓰려고 직접 만들어 공유했다."
category: "AI 에이전트·모델"
tags: ["Claude-Code", "desk-look", "플러그인(mod)", "터미널-UI", "diff/PR-패널", "AI-에이전트-도구"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36095"
original: "https://kyongsik-yoon.github.io/cc-desktop-mod/"
coverage: "full"
---

> 📌 2026-10-27 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36095) · [원문 기사](https://kyongsik-yoon.github.io/cc-desktop-mod/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

desk-look은 Claude Code CLI 터미널 화면을 Claude 데스크톱 앱의 Code 탭처럼 다시 그려 주는 MIT 라이선스 Claude Code 플러그인이다. 엔진은 건드리지 않고 화면 렌더링만 바꾸며, 작성자는 리눅스 터미널 환경에서 쓰려고 직접 만들어 공유했다.

## 핵심

- Claude Code에 추가된 mod(플러그인) 기능은 CLI를 개조하는 공식적인 방법이다. 작성자는 이것저것 설치하다 어떤 mod가 어떤 영향을 주는지 혼란스러워져, 원하는 구성요소를 직접 만들었다고 한다.
- 설치는 입력창에서 `/plugin install desk-look --marketplace KyongSik-Yoon/cc-desktop-mod` 한 줄이며, 업데이트는 `/desk-update`로 한다. 함수 훅 플러그인이라 화면 그리기만 바꾸고, 자체 네트워크 요청이나 MCP 호출은 하지 않는다. PR 정보는 사용자의 `gh`·`glab`으로 읽는다.
- 대화 기록 렌더링을 바꾼다. 프롬프트는 회색 말풍선으로, 연속된 도구 호출은 클릭해 펴는 한 줄로 접히며, 서브에이전트는 카드로 보인다. mermaid 블록은 상자 그림으로, 표는 전체 폭으로, `> [!NOTE]`는 알림 상자로 그려지고, 턴 끝에는 고친 파일 카드가 붙는다.
- 입력창 둘레에 저장소·브랜치, 변경 줄 수, PR 상태, 컨텍스트 사용률, 백그라운드 작업, Slack 안 읽은 메시지 칩이 있는 띠와 진행 띠가 붙는다. 칩을 누르면 해당 패널이 열린다.
- 오른쪽 패널은 일곱 개다. diff, 이번 턴 활동 타임라인, 백그라운드 작업, PR/MR, 마크다운 미리보기, 컨텍스트·사용량, 최근 세션이 있고, 패널은 Claude가 작업 중에도 바로 열린다. diff 줄에 댓글을 달면 다음 프롬프트와 함께 Claude에게 전달된다.
- Asana와 Slack 연동은 같은 마켓의 별도 플러그인 `desk-asana`, `desk-slack`으로 제공되며 필요한 것만 설치한다. 작성자는 Claude 스케줄 기능으로 Claude Code 업데이트를 따라가며 기능을 제안하고 구현하게 해 두었다고 한다.
- 둥근 말풍선 모서리와 이미지 미리보기는 kitty, Ghostty, iTerm2 3.5+에서 그려지고, tmux 3.3+와 herdr 안에서도 된다. 그 밖의 터미널에서는 둥근 테두리 말풍선으로 대체된다. 최신 Claude Code 기준으로 시험하며, 옛 버전에서는 기본 모양으로 돌아간다.

## 자세히

작성자는 일렉트론 기반 데스크톱 앱이 여러 세션을 동시에 돌리기에 무겁고 결국 CLI의 껍데기라서 터미널 CLI를 선호한다고 한다. 개발 환경이 리눅스라 맞는 도구를 찾지 못해 상태바만 꾸미던 중 mod 기능이 생겨 직접 만들게 되었다. mod 자체도 Claude에게 요청해 만들도록 안내하는 것이 공식 사용법이라고 한다.

설정은 `/config`에서 desk-look을 검색해 언어, 말풍선 위치, 컨텍스트 칩, 진행 띠 유지 시간 등을 고른다. 표시·패널·알림 문구는 Claude Code의 언어 설정(영어·한국어)을 따르고, 엔진의 테마 색을 쓰므로 밝은 테마와 어두운 테마를 모두 따른다.

## 왜 골랐나

AI 에이전트 개발 관점에서, Claude Code의 mod(플러그인·함수 훅) 구조로 에이전트 활동 타임라인, 서브에이전트 카드, 컨텍스트 사용량 시각화를 만든 사례라 에이전트 작업 관찰·관리 UI를 구상할 때 참고할 수 있다.
