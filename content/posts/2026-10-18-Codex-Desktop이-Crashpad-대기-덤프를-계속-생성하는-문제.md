---
title: "Codex Desktop이 Crashpad 대기 덤프를 계속 생성하는 문제"
date: "2026-10-18"
description: "macOS용 Codex Desktop과 통합 ChatGPT/Codex 앱에서 browser_crashpad_handler가 충돌과 재시작을 반복하거나 보관 제한 없이 Crashpad 대기 덤프를 쌓아, 앱은 정상 사용되는데도 디스크가 수십~수백 GB씩 소모된다는 GitHub 이슈 #25921 의 보고 모음이다. 모든 환경에 적용되는 확정 수정은 제공된 논의에 없다."
category: "AI 에이전트·모델"
tags: ["Codex-Desktop", "Crashpad", "browser_crashpad_handler", "디스크-고갈", "macOS", "Electron"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35565"
original: "https://github.com/openai/codex/issues/25921"
coverage: "partial"
---

> 📌 2026-10-18 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35565) · [원문 기사](https://github.com/openai/codex/issues/25921)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

macOS용 Codex Desktop과 통합 ChatGPT/Codex 앱에서 browser_crashpad_handler가 충돌과 재시작을 반복하거나 보관 제한 없이 Crashpad 대기 덤프를 쌓아, 앱은 정상 사용되는데도 디스크가 수십~수백 GB씩 소모된다는 GitHub 이슈 #25921 의 보고 모음이다. 모든 환경에 적용되는 확정 수정은 제공된 논의에 없다.

## 핵심

- 최초 보고(2026-06-02, Codex Desktop 26.527.60818, macOS 26.3.1, arm64)는 pending 디렉터리가 하루 4.9G, 54,504개 파일(.dmp와 _sidecar.json 각 27,252개)로 늘었다는 내용이다. 삭제 직후에도 6.4M, 70개 파일이 다시 생겨 생성이 계속됨을 보였다.
- 한 보고에서는 덤프에 browser_crashpad_handler 자체의 FATAL 오류가 들어 있었다. 위치는 exception_handler_server.cc:76 이고 오류는 mach_port_request_notification 의 invalid capability (20) 이다. 실패, 종료, 자기 감시 덤프 기록, 재실행이 반복되어 초당 덤프 1개, 하루 약 15GB가 생성됐다(228GB 디스크의 여유 공간이 0바이트가 된 사례).
- 누적 규모는 사례마다 크다. 61G/738,864개(6월 9일), 137.32GiB(6월 18일), 174.6GB(9월 2일)가 있고, 10월 15~17일 ChatGPT 26.1007.71326 에서는 기존 경로 180GB와 새 경로 143GB를 합쳐 약 323GB, 404만 개 파일을 측정했다. 앱 내부 경고는 없었다.
- 한 사례(9월 2일)는 핸들러 충돌 반복이 아니라 보관 제한 부재 자체의 문제였다. 약 161KB 덤프가 약 108만 개 쌓였고 디렉터리 링크 수가 65,535에 포화됐으며 4월 5일부터 하루 평균 약 1.1GB가 쌓였다. 8월 댓글에서는 Crashpad 의 pruner 가 앱에 연결되지 않았을 가능성도 제기됐다.
- 임시 조치는 모두 근본 해결이 아니다. 삭제는 생성이 즉시 재개되는 경우가 있고, pending 읽기 전용(chmod/chflags uchg)은 충돌 보고까지 막는다. --disable-breakpad 는 덤프를 막지 못했고, 핸들러 실행 차단은 'quit unexpectedly' 팝업을 반복시켰다. 10월 5일 보고자는 LaunchAgent 로 24시간 만료와 64그룹/256MiB 제한을 거는 방식을 썼다.
- 해결 여부는 불확실하다. 기여자는 macOS 26.5 이상에서 해결된 것 같다고 했고, 10월 18일에는 macOS 26.6.2 와 Codex 26.1013.40934 에서 영향이 없어 보인다는 댓글이 있다. 다만 macOS 수정만으로 충분한지, 앱 업데이트도 필요한지는 논의에서 확인되지 않았다.
- 요구 사항은 두 가지로 나뉜다. 하나는 연속 실패 시 재시도 간격을 늘리거나 생성률을 제한하는 것이다. 다른 하나는 pending 에 용량, 파일 수, TTL 제한과 자동 정리를 두는 것이다. 미니덤프에는 메모리, 경로, 프롬프트 등 사적 데이터가 있을 수 있어 공개 첨부를 피하라고 한다.

## 자세히

브라우저 기능(Browser Use)이 촉발 요인인지는 확인되지 않았다. 관련 플러그인을 비활성화하고 BROWSER_USE_AVAILABLE_BACKENDS 를 빈 값으로 설정해도, 재설치해도 생성은 멈추지 않았다. 7월 21일 사례에서는 빌드 5538과 5650의 Chromium 버전, 핸들러 Mach-O UUID가 같았으므로 이전 빌드로 되돌리는 것만으로는 피하기 어려워 보인다. 같은 Mach IPC 오류가 Claude Desktop 의 Tahoe 충돌과 VS Code 터미널 정지에서도 보고됐다는 언급이 있다.

기존 경로(~/Library/Application Support/com.openai.codex/web/Crashpad)와 새 경로(~/Library/Application Support/Codex/Crashpad)가 따로 쓰인 사례가 있다. 과거 누적분과 현재 반복 생성을 구분하려면 두 경로의 용량, 파일 수, 최신 파일 시각, 증가 여부를 따로 확인해야 한다. 10월 5일 사례의 보고자는 정리 시점의 빌드(26.930.21537)에서 대기 큐가 비어 있었다고 밝히면서도, 이를 과거 누적분으로 구분했고 근본 문제가 수정됐다고 주장하지는 않았다.

댓글에는 서드파티 스크립트와 /System/Library/Caches/com.apple.coresymbolicationd/data 의 충돌 관련 데이터 관찰도 있다. 후속 이슈로 #36982, #51361 이 언급된다. 하드웨어 손상 우려는 제기됐으나 이를 입증하는 측정은 없다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 코딩 에이전트 데스크톱 앱이 장시간 백그라운드로 도는 환경에서 크래시 리포터의 보관 제한과 재시도 백오프가 빠지면 사용자에게 보이지 않는 자원 고갈로 이어진다는 운영상의 사례다.
