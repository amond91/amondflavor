---
title: "Claude Code의 &quot;다시 묻지 않기&quot;, 공개 GitHub 파일 1,122개에 비밀번호 노출"
date: "2026-11-08"
description: "Apiiro 조사에 따르면 Claude Code에서 \"Yes, and don't ask again\"을 선택하면 비밀정보가 담긴 명령어 전체가 .claude/settings.local.json에 저장되고, 이 파일이 공개 GitHub에 커밋되어 자격증명이 노출되고 있다. 공개 저장소 103,617개의 로컬 설정 파일 104,185개 중 1,122개(1.1%)에서 서로 다른 비밀정보 1,379개가 발견됐다."
category: "AI 에이전트·모델"
tags: ["Claude-Code", "비밀정보-노출", "settings.local.json", "Apiiro", "AI-에이전트-보안", "gitleaks"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36784&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://apiiro.com/blog/coding-agents-leaking-secrets-github-dont-ask-again"
coverage: "full"
---

> 📌 2026-11-08 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36784&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://apiiro.com/blog/coding-agents-leaking-secrets-github-dont-ask-again)
> 근거: GeekNews 요약 페이지 — ⚠️ 원문 기사: HTTP Error 403: Forbidden

## 한 줄 요약

Apiiro 조사에 따르면 Claude Code에서 "Yes, and don't ask again"을 선택하면 비밀정보가 담긴 명령어 전체가 .claude/settings.local.json에 저장되고, 이 파일이 공개 GitHub에 커밋되어 자격증명이 노출되고 있다. 공개 저장소 103,617개의 로컬 설정 파일 104,185개 중 1,122개(1.1%)에서 서로 다른 비밀정보 1,379개가 발견됐다.

## 핵심

- 이 동작은 모델이 아니라 Claude Code 자체의 동작이다. Apiiro는 Opus 5.5와 Fable 5.1에서도 재현했다. Claude Code는 파일을 만들 때 개발자의 전역 Git 제외 목록에만 등록하고 저장소의 .gitignore에는 규칙을 넣지 않는다. 그래서 나중에 Git 저장소가 된 폴더, 다른 기기의 복제본, 이미 추적 중인 파일은 보호되지 않는다.
- 발견된 비밀정보 1,379개 중 알려진 제공업체 형식의 키가 636개, 공개 호스트와 함께 있는 비밀번호·토큰이 743개였다. 데이터베이스 비밀번호 379개와 Supabase service-role 키 122개가 포함됐고, 계정 내 모든 프로젝트를 제어하는 Supabase 계정 토큰도 103개 있었다. SSH 로그인 43개, GitHub/GitLab/npm 토큰 42개, AWS 키 23개, 개인 키 6개도 나왔다.
- 명령어에 직접 들어간 비밀번호(PGPASSWORD=, sshpass -p, mysql -p, curl Bearer 헤더 등)는 키 형식을 찾는 일반 탐지 도구가 놓치기 쉽다. 비밀정보가 있는 파일 1,122개 중 gitleaks는 564개, TruffleHog는 643개를 탐지했고, 215개는 두 도구 모두 놓쳤다.
- 노출은 오래 지속됐다. 비밀정보가 있는 저장소에서 파일의 최초 커밋 시점은 중앙값 기준 217일 전이었고, 저장소 74개의 포크 211개에도 같은 비밀정보가 복사돼 있었다. 현재 파일에는 없고 이전 버전에만 남은 비밀정보도 734개(509개 저장소)였다. 이력·공유 설정·대화 기록까지 합치면 1,994개 저장소에서 서로 다른 비밀정보 2,539개였다.
- 같은 문제는 다른 에이전트 파일에서도 나타났다. 공유 .claude/settings.json 76,011개 중 298개, 대화 기록 파일(.specstory, aider, Cursor 내보내기 등) 2,523개 중 120개 파일(87개 저장소), 메모리 파일 1,200개 중 9개 저장소, 인계·배포 문서 2,095개 중 33개 파일에서 발견됐다. Hugging Face Spaces와 GitLab.com에서도 확인됐다. 공개 저장소에 올라가지 않아도 로컬의 평문 JSON은 악성코드(예: Shai-Hulud 웜, 악성 Nx 패키지)의 수집 대상이 될 수 있다.
- 권고 사항은 다음과 같다. 노출된 자격증명은 파일 정리보다 먼저 교체한다. 추적 중인 파일은 git rm --cached로 추적을 해제하고 저장소의 .gitignore에 추가한다. 명령어에는 실제 값 대신 $DB_URL 같은 환경변수를 쓰고, 완전한 명령어 대신 Bash(npm run:*) 같은 패턴을 승인한다. 에이전트 폴더와 메모리 파일은 .env처럼 취급하고, 명령어 속 비밀번호와 Bearer 헤더까지 탐지하도록 커밋 전 검사와 CI를 구성한다.

## 자세히

조사는 2026년 11월 1~3일에 읽기 전용 GitHub 코드·커밋 검색으로 진행됐다. 코드 검색이 쿼리당 최대 1,000건이라는 제한을 피하려고 파일 크기별로 검색을 나눴다. 파일 이력은 43,120개 저장소를 대상으로 했으며, 이력에서 나온 수치는 하한값이다. 코딩 에이전트 16개의 승인 저장 방식을 문서로 비교한 결과, 기본 설정에서 승인한 전체 명령을 프로젝트 파일에 쓰는 것으로 확인된 것은 Claude Code뿐이었다.

분류 기준은 알려진 제공업체 키 형식, 그리고 공개 호스트와 같은 줄에 있는 일반 비밀번호·토큰이다. 자리표시자, 흔한 기본 비밀번호, 로컬·사설 호스트, 공개용 anon 키, 만료 토큰 등은 제외했다. 남은 2,774개 값을 독립 LLM 검토자가 마스킹된 문맥에서 검토했고, 규칙과 검토가 일치한 2,539개를 집계했다. 어떤 자격증명도 사용하거나 시험하지 않았으므로 현재 유효하다는 뜻은 아니다. 다만 Apiiro가 2026년 11월 2~3일 각 조직에 통보했을 때 한 기업은 해당 키가 실제 사용 가능했음을 확인했다.

이 메커니즘은 2025년 8월부터 사용자들이 GitHub 이슈(#5544, #11982, #13106, #91270)로 보고했지만 Anthropic의 답변은 없었고 3개는 비활성 또는 중복으로 자동 종료됐다. ironPeak는 2026년 2월 표본 100개에서 AI 설정 폴더가 있는 저장소의 2.4%가 이력에 비밀정보를 담고 있다고 측정했고, Lakera는 4월 npm 패키지 30개의 관련 파일 33개에서 자격증명을 발견했다.

## 왜 골랐나

AI 에이전트 개발에 직접 관련된다. 에이전트의 권한 승인 영속화, 대화 기록, 메모리 파일, 인계 문서가 비밀정보 유출 경로가 된다는 점은 에이전트 설계와 운영 시 참고할 만하다. amond의 로컬 .claude 설정과 메모리 파일에도 같은 점검을 적용할 수 있다.
