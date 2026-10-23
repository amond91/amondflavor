---
title: "OpenAI CLI - OpenAI REST API를 위한 공식 명령줄 도구"
date: "2026-10-23"
description: "OpenAI가 OpenAI REST API를 터미널과 셸 스크립트에서 바로 호출할 수 있는 공식 명령줄 도구 `openai` CLI를 공개했다. 별도 애플리케이션 없이 이미지 생성·파일 업로드·배치 제출·관리자 API 조회를 할 수 있고, Go로 작성되어 Apache 2.0 라이선스로 배포된다."
category: "AI 에이전트·모델"
tags: ["OpenAI-CLI", "REST-API", "배치-처리", "이미지-생성", "셸-자동화", "Go"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35818"
original: "https://github.com/openai/openai-cli"
coverage: "full"
---

> 📌 2026-10-23 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35818) · [원문 기사](https://github.com/openai/openai-cli)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

OpenAI가 OpenAI REST API를 터미널과 셸 스크립트에서 바로 호출할 수 있는 공식 명령줄 도구 `openai` CLI를 공개했다. 별도 애플리케이션 없이 이미지 생성·파일 업로드·배치 제출·관리자 API 조회를 할 수 있고, Go로 작성되어 Apache 2.0 라이선스로 배포된다.

## 핵심

- 명령 구조는 `openai <resource> [subresource...] <command> [flags...]` 형태의 리소스 기반이다. 기존 콜론 구분 명령(`openai admin:organization:usage completions`)도 계속 지원한다. 일반 엔드포인트는 `OPENAI_API_KEY`, 관리자 엔드포인트는 별도의 `OPENAI_ADMIN_KEY`를 쓴다.
- 주요 기능은 텍스트 응답 생성(`responses create`), 이미지 생성·편집, 파일 업로드·조회·다운로드, JSONL 배치 제출 후 `batches wait`로 완료 대기 및 결과 다운로드다. 이미지는 기본적으로 `~/Downloads/gpt-images/`에 저장되고, 지원 터미널에서는 미리보기가 된다.
- 출력은 기본이 읽기 쉬운 텍스트이며 파이프에서도 마찬가지다. 스크립트에서 파싱하려면 `--format json` 또는 `--format jsonl`을 명시해야 한다. `--transform`(GJSON 문법)과 `--transform-error`로 성공·오류 출력에서 필드를 추출할 수 있다.
- 신뢰 경계 관련 장치가 있다. JSON 입력의 `@파일명` 참조는 로컬 파일을 읽는데, 신뢰할 수 없는 JSON/YAML을 파이프로 받을 때는 `OPENAI_UNTRUSTED_STDIN=1`로 이 참조를 일반 데이터로 취급하게 할 수 있다. 자격 증명 헤더는 `OPENAI_CUSTOM_HEADERS`에 직접 설정하는 방식을 권장하며, 디버그 로그와 API 오류 세부 정보에는 민감 데이터가 포함될 수 있다.
- 사용자 지정 백엔드는 HTTPS가 필수이고(localhost·루프백만 HTTP 허용), mTLS는 베타다. HTTPS 프록시는 거부하고 HTTP·SOCKS 프록시만 지원한다.
- 편의 기능으로 셸 자동 완성과 명령 삽입(Ctrl+X 다음 Tab으로 예시 명령을 입력줄에 채우되 실행은 하지 않음)이 있다. 조건을 충족하는 첫 대화형 실행은 Bash·zsh·fish용 이미지 선택기 Tab 단축키를 별도 알림 없이 저장하며, 설정·제거 안내에 비활성화 방법이 있다.
- 업데이트 확인은 인자 없는 `openai`를 터미널에서 실행할 때 최대 7일에 한 번 알림을 표시하고, 공개 GitHub 릴리스 메타데이터를 하루 최대 한 번 조회한다. 자동 설치는 하지 않으며, `OPENAI_CLI_NO_UPDATE_CHECK=1`로 끌 수 있다.
- `images create-variation`은 폐지되었고, 변형 생성은 GPT Image 모델과 프롬프트를 지정한 `images edit`을 써야 한다.

## 자세히

README는 이 도구를 코딩 에이전트인 Codex CLI(`codex`)와 구분한다. 특정 API를 호출하거나 API 작업을 스크립트에 넣을 때는 `openai`를, 프로젝트의 코드를 읽고 고치는 작업에는 Codex CLI를 쓰라고 안내한다. 인프라 리소스(프로젝트, 서비스 계정, 레이트 리밋 등)를 코드로 관리하려면 공식 OpenAI Terraform provider를 쓰라는 안내도 있다.

설치는 GitHub Releases의 사전 빌드 바이너리(macOS·Linux·Windows), Homebrew(`brew install openai/tools/openai`), Go(`go install 'github.com/openai/openai-cli/cmd/openai@latest'`, Go 1.26 이상 필요) 중에서 고를 수 있다. 가이드 문서는 이미지, 파일, 배치, 오디오(음성 생성), 토크나이저(로컬 토큰 확인·계산), 웹훅 테스트, 출력 형식 등을 다룬다.

저장소 정보로는 570 커밋, 1.1k stars, 85 forks가 표시되어 있다. 버그와 기능 요청은 GitHub issues로 받지만, 저장소 협업자가 아닌 사람의 풀 리퀘스트는 받지 않는다. 개발용으로는 `./scripts/run`과 `./scripts/link`로 다른 OpenAI Go SDK 버전이나 로컬 사본을 연결해 테스트할 수 있다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 셸 스크립트나 에이전트의 도구 호출로 OpenAI API를 직접 부르는 수단이 생겼다. `OPENAI_UNTRUSTED_STDIN`처럼 외부 입력이 로컬 파일 읽기로 이어지는 것을 막는 설계는 에이전트 파이프라인의 신뢰 경계를 고민할 때 참고할 만하다.
