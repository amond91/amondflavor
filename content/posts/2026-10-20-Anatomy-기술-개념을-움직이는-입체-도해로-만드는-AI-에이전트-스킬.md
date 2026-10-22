---
title: "Anatomy - 기술 개념을 움직이는 입체 도해로 만드는 AI 에이전트 스킬"
date: "2026-10-20"
description: "Anatomy는 기술 개념(캐시, 연결 풀, 요청 제한 등)을 실제로 작동하는 기계·장치에 빗댄 인터랙티브 등각(isometric) 도해로 만들어 주는 에이전트 스킬이다. SVG를 중심으로 WebGL 효과와 베타 3D 모드를 결합하며, Claude Code·Codex·Antigravity·OpenCode·MiniMax Code에서 쓸 수 있고 MIT 라이선스로 공개되어 있다."
category: "AI 에이전트·모델"
tags: ["Agent-Skill", "인터랙티브-도해", "isometric-SVG", "WebGL", "Claude-Code", "시각화"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=35670"
original: "https://github.com/wheresryan22/anatomy"
coverage: "full"
---

> 📌 2026-10-20 · GeekNews · [원문 보기](https://news.hada.io/topic?id=35670) · [원문 기사](https://github.com/wheresryan22/anatomy)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Anatomy는 기술 개념(캐시, 연결 풀, 요청 제한 등)을 실제로 작동하는 기계·장치에 빗댄 인터랙티브 등각(isometric) 도해로 만들어 주는 에이전트 스킬이다. SVG를 중심으로 WebGL 효과와 베타 3D 모드를 결합하며, Claude Code·Codex·Antigravity·OpenCode·MiniMax Code에서 쓸 수 있고 MIT 라이선스로 공개되어 있다.

## 핵심

- 개념을 설명하는 그림이 아니라, 그 개념이 곧 작동 원리인 물리적 사물을 발명해 만든다. 예를 들어 캐시 키는 모든 핀이 맞아야 돌아가는 pin-tumbler 잠금장치로, 여러 canvas를 서비스하는 하나의 GPU context는 print head 하나가 16개 well을 오가는 gantry plotter로 표현된다.
- 그림은 세 종류다. SVG(4톤 음영의 등각 입체 부품, 독립 HTML·단독 .svg·React/Next.js 컴포넌트로 출력), SVG+WebGL(불꽃·배기·물·빛 반사 등을 앞뒤 SVG 사이 canvas에 셰이더로 렌더링), 3D beta(회전하거나 떨어지는 부품을 매 프레임 다시 투영·음영 처리)이다.
- 그림에 표시되는 모든 수치는 부품을 움직이는 동일한 작은 모델에서 나온다. 마우스·키보드 조작에 따라 움직임과 live readout이 함께 바뀐다.
- 검증 도구를 갖췄다. kit/audit.mjs가 부품 간 관통, 끝점의 고정 위치, 겹침의 깊이 순서를 검사하고, headless Chrome 기반 스크립트(capture.mjs, drive.mjs, lines.mjs, turn-check.mjs)가 스크린샷과 확대 검사를 수행한다. 스킬은 에이전트가 만든 PNG를 모두 읽어 보도록 지시한다.
- 설치는 `npx skills add wheresryan22/anatomy -g`이고 사용은 `/anatomy explain how a token bucket rate limiter works, interactive` 식이다. Codex에서는 `$anatomy`를 쓴다. API 키가 필요 없고 Node 내장 모듈만 사용하며, Node.js 20.10+ 와 Chrome/Chromium(검사용)이 필요하다.
- 완성 예제가 8개 포함된다: raptor-engine(11개 조립체로 분해되는 Raptor 2 엔진), ripple-tank, turning-dial(3D), dial-indicator(약 300줄의 최소 예제), desk-computer, arcade-cabinet, test-rig, spot-plate.
- 한계도 명시한다. 차트 라이브러리가 아니어서 축·계열·데이터 플롯이 없고, 기계·기구·도구·가구를 그리며 사람이나 정확한 지도는 그리지 않는다. 의도적으로 느려서 그림 하나에 긴 세션이 걸린다. 3D 모드는 beta이며 380부품 스트레스 예제는 1440px 폭에서 약 20fps였다.

## 자세히

작업 흐름은 SKILL.md에 정의되어 있다. 정확한 수치를 먼저 모으고, 사물을 발명하고, 부품 목록을 쓰고, 공간을 설계하고, kit으로 기하를 한 번 만든 뒤, 뒤에서 앞으로 그리고, 카드로 감싸 움직임을 넣고, 비평가처럼 검증한다. 사용자는 개념의 이름과 실제 수치(크기·개수·비율·임계값)를 주고, 결과물이 놓일 곳(HTML, SVG, React 컴포넌트, 라이트/다크 테마)을 말하는 것이 좋다고 안내한다.

결과물은 Node 빌드 스크립트와 그것이 만드는 페이지, 또는 React용 기하 모듈·서버 컴포넌트(정적 그림)·클라이언트 컴포넌트(움직임)로 사용자가 소스를 직접 소유한다. 일부 예제는 인쇄용 A4 PDF(포스터, 기술 도면, 2쪽 설명서)도 만든다. 만든 그림을 커뮤니티 갤러리에 올리는 publish 기능도 있다.

저장소는 별 18.5k, 포크 1.4k, 커밋 153개로 표시된다. 기여 규칙에는 코드에 주석을 쓰지 않는다는 항목과 기존 예제가 바이트 단위로 동일해야 한다는 항목이 있다.

## 왜 골랐나

AI 에이전트 개발 관점에서, 스킬(SKILL.md + 참조 문서 + 검증 스크립트)을 묶어 에이전트가 만든 결과를 스스로 검사하게 하는 구조의 사례로 참고할 만하다. 논문 발표나 기술 설명 자료에 쓸 인터랙티브 도해를 만드는 데도 활용할 수 있다.
