---
title: "Meta’s Muse Wonder and Spark Open, Educational Use of ChatGPT, Reflection's Partnership with Nvidia"
date: "2026-10-23"
description: "앤드루 응은 AI 네이티브 엔지니어링 팀을 이끌려면 People·Projects·Platforms·Processes 네 축에서 리더가 직접 앞장서야 한다고 말합니다. 뉴스로는 Meta의 Muse Wonder 계열 공개(Spark Open은 오픈 웨이트)와 OpenAI의 ChatGPT for Teens 신기능 및 Common Sense Media의 최저 등급 평가를 다룹니다."
category: "AI 에이전트·모델"
tags: ["AI-native-team", "Muse-Wonder", "Muse-Spark-Open", "오픈-웨이트", "ChatGPT-for-Teens", "Common-Sense-Media"]
source: "뉴스레터"
coverage: "partial"
---

> 📌 2026-10-23 · 뉴스레터
> 근거: 메일 본문

## 한 줄 요약

앤드루 응은 AI 네이티브 엔지니어링 팀을 이끌려면 People·Projects·Platforms·Processes 네 축에서 리더가 직접 앞장서야 한다고 말합니다. 뉴스로는 Meta의 Muse Wonder 계열 공개(Spark Open은 오픈 웨이트)와 OpenAI의 ChatGPT for Teens 신기능 및 Common Sense Media의 최저 등급 평가를 다룹니다.

## 핵심

- 앤드루 응의 머리글은 엔지니어링 리더를 위한 4P 프레임워크를 제시합니다. 목표는 AI를 쓰는 전통적 팀이 아니라, AI의 능력을 활용하고 병목을 우회하도록 설계된 AI 네이티브 팀입니다.
- People·Projects: 팀원의 AI 엔지니어링 역량 개발에 시간과 예산을 주고, 리더도 직접 코딩 에이전트를 써 봐야 한다고 합니다. 상향식 혁신과 함께 소수 유망 프로젝트는 top-down으로 지원하되, 효율·비용 절감뿐 아니라 신제품·성장 아이디어도 포함하라고 권합니다.
- Platforms·Processes: 에이전트가 쓸 수 있도록 데이터·저장소·문서·내부 도구를 정비하고, AI 코드 리뷰, CI, 샌드박스, 재사용 가능한 agent skill 공유 체계를 갖추라고 합니다. 명세가 명확할 때 소프트웨어 구축 비용이 크게 떨어졌으므로, 정보가 충분하고 권한이 위임된 팀으로 프로세스를 재설계하라는 주장입니다.
- Meta는 Muse Wonder Full(구독 전용, $50/월), Muse Wonder(API, $4/$20 per 1M 토큰), Muse Spark Open(오픈 웨이트, $1.25/$4.15)을 공개했습니다. 모두 텍스트·이미지·비디오·오디오를 입력받고 텍스트를 출력하며, 입력 최대 100만 토큰입니다.
- Artificial Analysis Intelligence Index에서 Wonder Full 56(공동 2위), Wonder 52(7위), Spark Open 39(오픈 웨이트 중 7위, 미국 오픈 모델 중 1위)를 기록했습니다. Spark Open은 816B 파라미터 MoE(활성 42B)이고 4비트로 약 1TB 메모리가 필요해 512GB Mac Studio 2대로 로컬 구동이 가능하다고 합니다. 라이선스는 매출 2,000만 달러까지 상업적 사용을 허용하고 'Built with Muse Spark Open' 표기를 요구하며, Wonder 계열은 비공개입니다.
- OpenAI는 18세 미만용 ChatGPT for Teens에 College Planner, 플래시카드, 노트 기반 퀴즈, Study Mode 등을 추가한다고 발표했습니다. 같은 날 Common Sense Media의 Youth AI Safety Institute는 이를 최저 등급인 'Unacceptable Risk'로 평가하고, 독립 검증 전까지 성인 전용으로 제한하라고 촉구했습니다.
- 뉴스레터 끝부분에 DeepLearning.AI의 AI Dev NYC(11/30–12/1) 홍보가 있고, 구독자에게 20% 할인을 제공합니다.

## 자세히

머리글에서 응은 팀 구성 단계에 따라 접근이 다르다고 말합니다. 2~3명의 새 팀이면 처음부터 AI 네이티브 인력으로 꾸릴 수 있지만, 대기업은 기존 팀을 앞으로 이끌 방법을 찾아야 합니다. 리더가 직접 만들어 보면 기술·사업 판단의 직관이 좋아진다는 점을 자전거 비유로 설명합니다. 프로세스 부분에서는 엔지니어가 제품 관리 결정과 때로는 마케팅·디자인에도 참여한다고 하며, 기밀 유출·대형 장애·데이터 손실 같은 큰 위험에 대한 최소한의 안전장치만 두라고 합니다. 팀 간 조율은 여전히 오래된 문제이고, 팀별 에이전트끼리 정보를 공유하게 하는 방식이 유망하다고 하지만 관행은 아직 형성 중이라고 밝힙니다.

Meta 기사에서 Muse Wonder 계열의 모델 카드는 아키텍처와 학습 방법을 거의 공개하지 않습니다. 지식 컷오프와 Wonder 계열의 파라미터·아키텍처, 학습 데이터도 비공개입니다. Artificial Analysis는 Wonder 계열의 지식 점수가 낮지만 환각률이 낮아 일부 상쇄된다고 평가했습니다(AA-Omniscience Index에서 Wonder Full 30으로 8위, Wonder 25로 10위). 비용 면에서 Wonder는 Intelligence Index 과제당 $4.52로 현재 모델 중 5번째로 비싸고, Spark Open은 과제당 $0.84입니다. 필자는 비디오·오디오 입력을 처리하는 다른 주요 모델이 없다는 점을 의의로 꼽고, Meta의 오픈 웨이트 복귀를 환영합니다.

ChatGPT for Teens는 별도 모델이 아니라 13~17세로 등록되었거나 연령 예측 모델이 미성년자로 판단한 계정에 켜지는 설정·분류기·프롬프트·인터페이스의 묶음입니다. OpenAI는 청소년의 하루 평균 사용이 15분 미만이라고 밝혔습니다. Common Sense Media는 4,000건이 넘는 프롬프트로 출시 전후를 테스트했지만, 제공된 원문은 이 대목에서 잘려 결과 상세는 확인할 수 없습니다.

## 왜 골랐나

AI 에이전트 개발 관점에서 머리글의 Platforms(에이전트가 접근할 문서·도구·skill 공유 체계)와 Processes(에이전트 시대의 병목 재설계) 논의가 직접 맞닿습니다. 기술경영·기술정책 연구 관점에서는 Meta의 폐쇄·개방 전략 변화와 라이선스 조건, 청소년용 AI 안전 규제·평가 논쟁이 사례가 될 수 있습니다.
