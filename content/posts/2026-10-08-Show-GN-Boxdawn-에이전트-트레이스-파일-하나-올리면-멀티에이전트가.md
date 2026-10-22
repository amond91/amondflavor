---
title: "Show GN: Boxdawn - 에이전트 트레이스 파일 하나 올리면 멀티에이전트가 같은 일을 두 번 한 지점을 찾습니다"
date: "2026-10-08"
description: "Boxdawn은 멀티에이전트 트레이스 파일 하나를 올리거나 CLI로 돌리면, 여러 에이전트가 같은 파일을 반복해서 읽는 등 중복 작업 지점을 찾아 보여주는 오픈소스 도구다. 찾아서 보여주기만 하고 수정 패치는 내지 않는다."
category: "AI 에이전트·모델"
tags: ["Boxdawn", "멀티에이전트", "에이전트-트레이스", "중복-탐지", "OpenTelemetry", "Claude-Code"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=34953"
original: "https://boxdawn.com/analyze"
coverage: "partial"
---

> 📌 2026-10-08 · GeekNews · [원문 보기](https://news.hada.io/topic?id=34953) · [원문 기사](https://boxdawn.com/analyze)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Boxdawn은 멀티에이전트 트레이스 파일 하나를 올리거나 CLI로 돌리면, 여러 에이전트가 같은 파일을 반복해서 읽는 등 중복 작업 지점을 찾아 보여주는 오픈소스 도구다. 찾아서 보여주기만 하고 수정 패치는 내지 않는다.

## 핵심

- 멀티에이전트에서 에이전트 A가 읽은 파일을 B, C가 또 읽는 식의 중복은 개별 로그로는 정상으로 보이고, 비용은 그 사이에서 나온다. 작성자는 같은 파일을 네 번 읽은 것을 눈으로 발견한 뒤 이를 자동으로 세어주는 도구를 찾지 못해 직접 만들었다.
- 사용법은 boxdawn.com/analyze 에 트레이스 파일(.json 또는 .jsonl, 최대 10 MB)을 올리는 것으로, 가입이나 로그인이 필요 없다. 로컬에서는 `pip install boxdawn` 후 `boxdawn analyze <세션 파일>` 두 줄로 실행한다. 웹 페이지는 파일 사본을 보관하지 않으며 임시 디렉터리에서 처리한 뒤 응답이 끝나면 삭제한다고 밝힌다.
- 입력 형식은 Claude Code 세션 로그(계측 없이 바로 가능), OpenTelemetry SDK JSON, OpenInference(Phoenix/TRAIL)이며 자동으로 판별한다. 이미 쌓인 트레이스로 바로 분석되게 하는 것이 설계 목표였다.
- 판정은 두 단계다. 먼저 sha256 동일성, 중간 쓰기 없이 같은 대상을 다시 읽었는지 등 결정적인 구조 판정을 하고, 애매한 것만 선택 단계인 의미 판정으로 넘긴다. 의미 판정은 API 키를 넣지 않으면 돌지 않는다. 패턴은 repeat, context_resend, redundant_read, duplicate_creation, semantic_duplicate 다섯 가지다.
- 한계도 공개했다. AutoGen은 계측기가 LLM 스팬을 내보내지 않아 실제 워크로드에서 0건이 나와 실패했고, 그 결과표를 README에 그대로 뒀다. LlamaIndex, OpenAI Agents SDK, Anthropic SDK는 통과했다. 의미 판정 임계값은 합성 데이터로 정했고 실데이터로 재보정하지 않았다. Cursor와 Codex 세션은 아직 읽지 못한다.
- 공개 코퍼스 네 종의 17,881개 트레이스에서 돌려봤고, 실제 운영 중인 멀티에이전트에서의 결과와 특히 낭비가 아닌데 낭비로 잡힌 오탐 사례를 GitHub 이슈로 받고 싶다고 요청한다.

## 자세히

이 글은 GeekNews의 Show GN 게시물로, 도구의 동기, 사용법, 판정 방식, 미지원 항목을 짧게 정리한 소개글이다. 핵심 문제의식은 비용 낭비가 개별 에이전트가 아니라 에이전트 사이의 중복에서 생긴다는 점이다.

판정은 결정적인 구조 규칙을 우선하고 모호한 경우에만 선택적으로 의미 판정을 쓰는 구조다. 웹 분석 페이지는 결정적 탐지기(deterministic detectors)만 돌린다고 적혀 있어, 의미 판정은 로컬에서 API 키를 넣었을 때만 쓰이는 것으로 보인다.

원문 기사로 가져온 boxdawn.com/analyze 페이지는 업로드 안내문뿐이어서 탐지 로직의 세부는 확인할 수 없었다. 코드와 README는 github.com/boxdawn/boxdawn 에 있다고 안내돼 있으나 이번 입력에는 포함되지 않았다.

## 왜 골랐나

AI 에이전트 개발 관심사와 직접 맞닿는다. Claude Code 세션 로그(~/.claude/projects/.../session.jsonl)를 그대로 분석할 수 있어 멀티에이전트 워크플로의 중복 읽기와 컨텍스트 재전송으로 인한 비용을 점검하는 데 쓸 수 있다.
