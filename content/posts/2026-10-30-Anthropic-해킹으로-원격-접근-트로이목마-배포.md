---
title: "Anthropic 해킹으로 원격 접근 트로이목마 배포"
date: "2026-10-30"
description: "Cursor 확장 마켓플레이스의 공식 Claude Code 확장이 변조돼 자동 업데이트로 배포된 공급망 공격이 발생했다. 악성코드는 SSH 키 추가·역방향 터널·자동 실행 스크립트로 지속적 원격 접근을 시도했으며, Anthropic은 침해 지점이 Cursor 마켓플레이스이고 자사 시스템과 공식 배포 채널은 영향받지 않았다고 밝혔다."
category: "AI 에이전트·모델"
tags: ["공급망-공격", "Cursor", "Claude-Code", "SSH-백도어", "자동-업데이트", "KQL-탐지"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36288"
original: "https://doublepulsar.com/anthropic-hacked-to-deliver-remote-access-trojan-to-claude-developer-laptops-worldwide-1148a92baae2"
coverage: "summary-only"
---

> 📌 2026-10-30 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36288) · [원문 기사](https://doublepulsar.com/anthropic-hacked-to-deliver-remote-access-trojan-to-claude-developer-laptops-worldwide-1148a92baae2)
> 근거: GeekNews 요약 페이지 — ⚠️ 원문 기사: HTTP Error 403: Forbidden

## 한 줄 요약

Cursor 확장 마켓플레이스의 공식 Claude Code 확장이 변조돼 자동 업데이트로 배포된 공급망 공격이 발생했다. 악성코드는 SSH 키 추가·역방향 터널·자동 실행 스크립트로 지속적 원격 접근을 시도했으며, Anthropic은 침해 지점이 Cursor 마켓플레이스이고 자사 시스템과 공식 배포 채널은 영향받지 않았다고 밝혔다.

## 핵심

- Kevin Beaumont이 Cursor 마켓플레이스에서 변조된 Claude Code 확장이 자동 업데이트로 설치된 사건을 정리했고, Phorion이 단말의 악성 활동을 발견해 분석을 공개했다.
- 최신 Cursor 공지는 macOS·Windows용 2.1.312와 2.1.313 변조본을 영향 대상으로 명시한다. 정상 버전 번호를 재사용한 변조본도 있어 버전 번호만으로는 판단하기 어렵다.
- 악성코드는 공격자의 SSH 키를 사용자 인증 파일에 추가하고 외부 서버를 경유하는 역방향 터널을 구성하려 한다. macOS는 사용자 LaunchAgent, Windows는 SYSTEM 권한 예약 작업(실패 시 시작 프로그램 스크립트)으로 접근을 유지하며, 약 60초마다 터널 연결을 재시도한다.
- 호스트·사용자 정보와 백도어 설치 상태를 8099번 포트의 /beacon 으로 HTTP POST 전송한다. 환경변수는 값이 아닌 이름만 수집한다.
- SSH 활성화 단계는 권한 부족과 코드 오류로 실패할 수 있지만, 이 실패가 키 추가·정보 전송·지속성 설치까지 막지는 않는다.
- 새 확장으로 업데이트해도 기존 SSH 백도어와 자동 실행 스크립트는 남을 수 있다. Cursor는 악성 배포본 제거, 마켓플레이스 키 교체, 공격자 서버 차단을 진행했고 감염 확인·복구 스크립트와 인증 정보·세션 교체 안내를 공개했다. Beaumont은 Microsoft Defender for Endpoint용 KQL 탐지 쿼리를 공개했다.
- Anthropic은 공격자가 Cursor 마켓플레이스 접근권을 이용했으며 자사는 변조본을 제작·게시하지 않았다고 밝혔다. 접근권을 얻은 구체적 방법은 공개되지 않았다.

## 자세히

원문(Beaumont)은 처음에 Anthropic 침해로 표현했다가 "침해 지점은 Cursor"라는 Anthropic의 설명을 추가해 수정됐다. 제목의 "Anthropic 해킹"은 이 수정 전 표현에 가깝고, 후속 설명에 따르면 Anthropic 자체가 해킹된 것이 아니라 제3자 마켓플레이스 배포 경로가 악용된 사건이다.

원문에 실린 초기 파일 해시 일부는 후속 공지에서 갱신됐다. 따라서 침해 지표는 최신 Cursor 공식 공지를 기준으로 확인해야 한다.

## 왜 골랐나

AI 에이전트 개발 도구(IDE 확장)가 자동 업데이트 경로를 통해 공급망 공격의 표적이 된 사례다. 에이전트 개발 환경의 확장·마켓플레이스 신뢰 문제와 연결된다.
