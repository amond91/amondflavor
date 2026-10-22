---
title: "Anthropic의 Scion이 Mononoke를 베꼈다는 비판에 대한 추가 설명"
date: "2026-10-16"
description: "Mononoke의 Bonsai 데이터 모델 설계를 이끌었던 엔지니어가, Anthropic의 Scion이 Mononoke를 의도적으로 모방했고 출처 표기가 부족하다고 후속 비판을 냈다. 이후 Anthropic 블로그에 출처 표기가 추가되었고, 작성자는 수정은 인정하되 당시 비판은 유지한다고 밝혔다."
category: "AI 에이전트·모델"
tags: ["Scion", "Mononoke", "Bonsai", "출처-표기", "Sapling", "LLM-재구축"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35434"
original: "https://sunshowers.io/posts/anthropic-mononoke-2/"
coverage: "full"
---

> 📌 2026-10-16 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35434) · [원문 기사](https://sunshowers.io/posts/anthropic-mononoke-2/)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Mononoke의 Bonsai 데이터 모델 설계를 이끌었던 엔지니어가, Anthropic의 Scion이 Mononoke를 의도적으로 모방했고 출처 표기가 부족하다고 후속 비판을 냈다. 이후 Anthropic 블로그에 출처 표기가 추가되었고, 작성자는 수정은 인정하되 당시 비판은 유지한다고 밝혔다.

## 핵심

- 작성자는 전날 글을 쓴 뒤 비공개로 받은 확인으로 Scion이 Mononoke를 의도적이고 노골적으로 베꼈으며 참여 인력이 겹친다고 판단했다. 다만 그 확인의 구체적 내용은 공개하지 않았다.
- 유사성은 파생 데이터(derived data)에 그치지 않고 전체 데이터 모델과 주변 구조에 걸친다. 작성자는 Scion changeset이 2022년 Oxide 자료에서 설명한 Mononoke의 Bonsai changeset과 "정확히 같다"고 주장한다. 이름 "Scion"이 "Bonsai"의 음절을 뒤집고 "B"를 뺀 것과 같다는 점도 짚지만, 우연일 수 있다고 덧붙인다.
- 핵심 쟁점은 복제가 아니라 출처 표시다. 좋은 아이디어를 복제하는 것은 문제가 아니며, 출처를 지우는 것이 문제라고 본다. Sapling의 "힘들게 얻은 교훈"만 언급한 것은 불충분하다고 한다. Sapling은 클라이언트 sl일 수도 전체 시스템일 수도 있는데, Meta 소스 제어 팀 출신이면 그 차이를 안다는 이유다.
- Bonsai 모델은 Git식 모델이 지배적이던 2018년에 write-ahead log 방식의 Merkle DAG를 택한 비주류 설계였고, 서버 측 리베이스 같은 어려운 문제를 풀어야 했다. 약 10년이 지난 지금도 초기 설계 범위를 넘어 쓰이고 있다.
- "LLM이 5개월 동안 시스템을 만들고 운영 문제까지 해결했다"와 "Meta 팀이 10년간 만든 시스템을 LLM으로 5개월 만에 재구축했다"는 서로 다른 성과라고 본다. 그래도 후자 역시 상당한 성취이며, 처음부터 Mononoke 복제라고 밝혔어도 주목할 만했다고 한다.
- 현재 Mononoke를 유지하는 팀의 기여도 인정받아야 하며, 의도적 복제와 타인 성과의 자기 성과화는 이 팀에 가장 직접적인 타격이라고 한다.
- 상단 업데이트에 따르면 Anthropic의 Nick G.가 Hacker News에서 답변했고 Anthropic 블로그에 충분한 출처 표기가 추가되었다. 작성자는 각 글을 쓴 시점의 비판은 유지한다.

## 자세히

이 글은 앞선 글("Anthropic's Scion is heavily inspired by Mononoke")의 후속편이다. 앞선 글에서는 유사성이 대체로 의도하지 않은 결과이고 출처 누락도 충분히 확인하지 않은 탓일 수 있다고 열어 두었다. 이번 글에서는 비공개로 받은 확인과 인력 중첩을 근거로 그 입장을 바꾸었다.

Blame용 인덱스처럼 파생 데이터의 종류가 같은 것은 Bonsai 모델에서는 자연스러운 결과라고 작성자도 인정한다. 비판의 초점은 인덱스의 존재가 아니라 Meta 소스 제어 팀이 쓰던 용어와 사고방식까지 닮았다는 점이다. 서버 측 API 유사성은 Mononoke 이전 시스템에도 있었을 것으로 보아 독창성을 주장하지 않으며, Claude에 Mononoke를 참조해 재구축하게 하면 비슷한 설계가 나왔을 것이라고 추측한다. Lore VCS는 Mononoke에서 영감을 받았지만 여러 면에서 다르다고 한다. 반면 Scion은 훨씬 직접적으로 닮았다고 본다.

작성자는 Bluesky와 LinkedIn에 처음 문제를 제기한 뒤 이틀 동안 원문 수정이 없어 Anthropic의 공개 엔지니어링 발표를 강하게 불신하게 되었다고 쓴다. 이후 상단 업데이트로 출처 표기가 추가된 것을 확인했다. 이 글의 주장은 한 당사자의 일방적 서술이며, Anthropic 쪽 답변(Hacker News)이나 Scion 원문은 이 자료에 포함되어 있지 않다.

## 왜 골랐나

AI 에이전트·LLM으로 기존 시스템을 재구축할 때의 출처 표기와 성과 서술 문제를 다룬다. AI 에이전트 개발과 기술혁신·지식재산(선행 기술의 인정과 모방) 연구 관점에서 참고할 만한 사례다.
