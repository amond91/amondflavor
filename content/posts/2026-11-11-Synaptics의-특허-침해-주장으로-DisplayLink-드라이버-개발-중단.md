---
title: "Synaptics의 특허 침해 주장으로 DisplayLink 드라이버 개발 중단"
date: "2026-11-11"
description: "Linux용 DisplayLink 도킹 스테이션 드라이버(Vino)를 개발하던 개인 개발자 Mike Lothian이 Synaptics로부터 특허 침해 경고, 중단 요구서, 2만 5,000달러 청구 통보를 받고 저장소를 내리고 커널 제출을 중단했다. Open Invention Network(OIN)의 보호도 코덱 제외 규정 때문에 적용되지 않는다는 판단이 나왔다."
category: "기술경영·산업"
tags: ["Synaptics", "DisplayLink", "특허-침해", "Open-Invention-Network", "Linux-드라이버", "소프트웨어-특허"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36962&utm_source=geeknews&utm_medium=rss&utm_campaign=news"
original: "https://lwn.net/SubscriberLink/1102549/acbc92e00cee3a3f/"
coverage: "partial"
---

> 📌 2026-11-11 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36962&utm_source=geeknews&utm_medium=rss&utm_campaign=news) · [원문 기사](https://lwn.net/SubscriberLink/1102549/acbc92e00cee3a3f/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Linux용 DisplayLink 도킹 스테이션 드라이버(Vino)를 개발하던 개인 개발자 Mike Lothian이 Synaptics로부터 특허 침해 경고, 중단 요구서, 2만 5,000달러 청구 통보를 받고 저장소를 내리고 커널 제출을 중단했다. Open Invention Network(OIN)의 보호도 코덱 제외 규정 때문에 적용되지 않는다는 판단이 나왔다.

## 핵심

- Lothian은 Dell 도킹 스테이션(DisplayLink 칩셋)이 Linux에서 성능이 낮고 드라이버가 계속 충돌하자, Claude 구독을 활용해 Rust 기반 DRM/KMS 드라이버 Vino를 개발했다. 6월 17일 dri-devel 메일링 리스트에 첫 패치 시리즈를 RFC로 제출했다.
- Synaptics는 7월 20일 DisplayLink가 개방형 표준이 아니며 특허로 보호된다고 알렸다. 독자 작성이나 클린룸 구현도 특허 발명을 구현하면 침해를 피하지 못할 수 있다고 경고했다.
- 8월 17일 중단 요구서에는 특허 125건의 목록이 붙었지만 어느 특허가 침해되는지는 특정하지 않았다. 요구사항은 8가지였고, 개발 중단과 제출 자료 철회, 게시 위치의 서면 공개를 포함했다.
- 10월 5일자 후속 서한에는 영국 특허 2건(GB2484736B, GB2485613B)의 청구항 대응표가 포함됐다. Synaptics는 요구 불이행 시 2만 5,000달러를 청구하고 이행할 때까지 매주 2만 5,000달러씩 늘리겠다고 통보했다. 최종 기한은 10월 18일로 연장됐다.
- Synaptics는 OIN 회원이지만, Synaptics는 DisplayLink가 OIN 회원이 아니며 그 기술은 OIN을 통해 라이선스되지 않는다고 주장했다. OIN은 Linux System Definition이 코덱을 명시적 목록에 없으면 제외하므로 Lothian의 커널 기여에는 보호가 적용되지 않는다고 판단했다. CEO Keith Bergelt도 10월 23일 이를 확인했다.
- Lothian은 GitHub 저장소를 내렸고 Vino는 커널에 제출하지 않고 로컬에만 두기로 했다. 법적 방어 비용을 감당할 수 없다는 이유다. 특허 주장에 영향을 받지 않는 일부 구성요소는 계속 작업할 수 있다.

## 자세히

Synaptics가 제공하는 DisplayLink Manager(DLM)는 독점 소프트웨어로 x86 바이너리만 배포되며, 함께 제공되는 EVDI는 오픈소스 외부 커널 모듈이다. 회사는 DL3라는 독점 그래픽 기술을 소스 비공개 사유로 밝혔다. Lothian은 8월 26일 세 번째 버전을 제출했는데, 이 버전은 제조사 소프트웨어 없이 3세대에 걸친 DL3 도크를 제어할 수 있었다. 이후 청구항 대응표와 OIN 약정의 양립 여부를 물었지만 Synaptics는 금전 보상 요구로 대응을 강화했다.

기사 댓글(LWN 독자 토론)에서는 몇 가지 논점이 나왔다. 하나는 Synaptics가 경쟁우위 훼손을 주장한 것이 특허(공개를 전제)가 아니라 영업비밀의 논리에 가깝다는 지적이다. 다른 하나는 역공학으로 상세 명세서만 작성하고 각 사용자가 LLM으로 구현하게 하는 우회 방법이 논의됐으나, 이를 공유 정신에 반하는 포기라고 비판하는 반론도 있었다. 이는 독자 의견이며 기사 본문의 주장은 아니다.

## 왜 골랐나

소프트웨어 특허가 오픈소스 개발과 상호운용성을 막는 사례로, 특허 전략과 기술경영·기술정책 연구에 참고할 만하다. 특히 OIN 같은 방어적 특허 풀의 보호 범위 한계(코덱 제외)와, LLM 보조 개발이 특허 리스크를 해소하지 못한다는 점이 AI 에이전트 개발과도 맞닿는다.
