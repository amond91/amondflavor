---
title: "Show GN: Translate Book: arXiv - arXiv 논문을 원문 구조 그대로 번역본으로 변환해 주는 Claude Code 스킬"
date: "2026-10-27"
description: "Translate Book: arXiv는 arXiv 논문의 PDF가 아니라 LaTeX 원본을 받아 번역해, 수식·표·그림·식 번호·상호 참조 등 원문 구조를 그대로 살린 번역본(PDF, EPUB, DOCX)을 만들어 주는 Claude Code 스킬이다. deusyu/translate-book을 포크해 arXiv LaTeX 경로와 결과 검사 부분을 새로 만들었다."
category: "AI 에이전트·모델"
tags: ["Claude-Code-스킬", "arXiv", "LaTeX-번역", "병렬-sub-agent", "용어집", "Pandoc"]
source: "GeekNews"
link: "https://news.hada.io/topic?id=36087"
original: "https://github.com/kcy4334-lgtm/translate-book-arxiv"
coverage: "partial"
---

> 📌 2026-10-27 · GeekNews · [원문 보기](https://news.hada.io/topic?id=36087) · [원문 기사](https://github.com/kcy4334-lgtm/translate-book-arxiv)
> 근거: GeekNews 요약 페이지 + 원문 기사

## 한 줄 요약

Translate Book: arXiv는 arXiv 논문의 PDF가 아니라 LaTeX 원본을 받아 번역해, 수식·표·그림·식 번호·상호 참조 등 원문 구조를 그대로 살린 번역본(PDF, EPUB, DOCX)을 만들어 주는 Claude Code 스킬이다. deusyu/translate-book을 포크해 arXiv LaTeX 경로와 결과 검사 부분을 새로 만들었다.

## 핵심

- 일반 번역기는 수식·그림·표가 깨지고 위치가 어긋나 가독성이 떨어진다. 이 스킬은 arXiv의 LaTeX 원본을 받아 번역하므로 결과물이 '번역된 글'이 아니라 '번역된 논문'이 된다. 절 구성과 번호, 수식과 식 번호, 표, 캡션 달린 그림, 인용·참고문헌, '식 (3)'·'그림 2' 같은 상호 참조가 유지된다.
- 사용법은 Claude Code에서 '이 논문 한국어로 번역해줘'라고 요청하는 것이다. README 기준 설치는 `npx skills add kcy4334-lgtm/translate-book-arxiv -a claude-code -g`이며, Codex와 OpenClaw도 지원한다. 스킬은 PDF 첫 페이지에서 arXiv 논문임을 인식하고, 소스를 내려받기 전에 사용자에게 먼저 묻는다.
- 번역 파이프라인은 LaTeX를 평탄화하고 논문이 가진 매크로를 .sty에서 풀어낸 뒤 pandoc으로 수식이 살아 있는 Markdown을 만든다. 이를 약 6,000자 단위 chunk로 나눠 병렬 sub-agent(기본 8개씩)가 번역한다. 참고문헌 목록은 번역하지 않고 그대로 복사한다.
- 오류 방지 장치가 있다. chunk끼리 용어집을 공유해 용어 불일치(예: attention을 '어텐션'과 '주의'로 섞어 쓰는 일)를 막는다. 표의 '97.9 ± 0.4'에서 ±가 빠지는 식의 조용한 누락은 verify_chunk.py 등 검사 코드로 잡는다. README에는 이 밖에도 여러 probe 스크립트가 나열되어 있고, 테스트는 2,705개라고 적혀 있다.
- 출력은 인쇄 가능한 PDF, EPUB, DOCX다. 대상 언어는 플래그로 고르며 zh, en, ja, ko, fr, de, es 등을 지원한다. 인쇄 레이아웃은 한국어 기준으로 맞췄다. 예시로 arXiv:2609.11801 논문을 번역한 31쪽짜리 한국어 책이 공개되어 있다.
- 머신러닝뿐 아니라 물리, 정수론, 양자컴퓨팅 등 26편의 arXiv 논문으로 시험했고 26편 모두 책으로 나왔다고 한다. 필요한 것은 Python, Pandoc, Chrome 또는 Edge이고, EPUB 출력에는 Calibre가 추가로 필요하다. 시작할 때 빠진 도구를 알려 준다.
- 한계도 밝혀져 있다. 여러 에이전트가 논문 전체를 번역하므로 Claude 사용량이 적지 않으며, 작성자는 계속 최적화할 예정이라고 한다. Anthropic 공식 플러그인 디렉터리에도 올라가 있으나 업데이트가 검토 중이어서 최신 버전은 GitHub에서 받는 편이 낫다고 한다.

## 자세히

README는 이 스킬이 네트워크로 가져오는 것이 arXiv 논문의 LaTeX 소스(https://arxiv.org/e-print/<id>) 하나뿐이고, 가져오기 전에 사용자에게 묻는다고 밝힌다. 텔레메트리나 업로드는 없고, 번역은 사용자의 에이전트 모델이 sub-agent로 수행한다. 작업 파일과 결과물은 현재 디렉터리의 `<book>_temp/`에 저장된다. 실행 중 얻은 지식은 스킬 폴더 안의 KNOWLEDGE.md, KNOWHOW.md, REFEREE.md, corpus/shapes.json에 누적된다.

품질 관리 측면에서는 chunk마다 원문·용어집·인용 chunk와 대조해 검증한 뒤에야 병합한다. 번역 전 용어집을 만들고 chunk별 용어표를 프롬프트에 제약 조건으로 넣는다. 해시를 기록해 재실행 때 바뀐 부분만 다시 번역한다. 새로운 LaTeX 구문이 나오면 테스트가 실패하도록 해서, 한 번 만난 구문은 다음 논문에서 처리되도록 한다. 또한 old-man, question-monster, fast-finder, referee라는 네 가지 advisor sub-agent가 로그를 읽고 오류 원인을 짚는다.

작성자는 연구실에서 논문을 읽을 때 써 보고, 깨지는 논문이 있으면 이슈로 알려 달라고 요청한다. 제공된 원문은 README 중간(프로젝트 구조 표)에서 잘려 있어 이후 내용은 확인하지 못했다.

## 왜 골랐나

논문을 읽고 연구하는 amond 의 작업 흐름에 직접 닿는다. 영어 arXiv 논문을 수식·표·번호 체계가 유지된 한국어 책으로 만들 수 있어서, AI·기술혁신 분야 논문을 읽을 때 쓸 수 있다. 또 chunk 병렬 번역, 공유 용어집, 검증 스크립트로 에이전트의 조용한 오류를 잡는 구조는 AI 에이전트 개발 설계의 참고 사례가 된다.
