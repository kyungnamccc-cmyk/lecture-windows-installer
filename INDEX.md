---
title: "vault 통합 INDEX — 윤용식"
created: 2026-04-16
updated: 2026-04-16
tags: [INDEX, MOC, vault, MW1-5, 성우하이텍]
---

# vault 통합 INDEX — 윤용식

> 이 파일은 vault 전체의 지도입니다.
> AI에게 "INDEX.md 읽고 답해줘"라고 하면 vault 전체 맥락을 한 번에 전달합니다.
> 소속: 성우하이텍 A-RnD (CAE / 배터리 분석)

---

## vault 구조 개요

이 vault는 [[MY-VAULT-NAVIGATION|MY-VAULT-NAVIGATION.md]]에 정의된 구조로 운영됩니다.

```
vault/
├── INDEX.md              ← 지금 이 파일 (vault 전체 지도)
├── MY-VAULT-NAVIGATION.md ← vault 구조 정의
├── mw1-5-wiki/           ← 1-5주차 강의 위키
│   ├── 주차별/           ← MW1~MW5 요약
│   └── Concepts/         ← 핵심 개념 정의
├── Mine/                 ← (예정) 내가 작성한 자료
│   ├── Reports/          ← 팀 공유용 보고서
│   ├── Analysis/         ← CAE 해석 메모
│   └── Memos/
├── Library/              ← (예정) 외부에서 받은 자료
│   ├── Papers/
│   ├── Specs/
│   └── References/
└── External-Research/    ← 외부 출처 아카이빙
```

---

## 1-5주차 학습 요약

### 주차별 연결지도

| 주차 | 주제 | 핵심 산출물 | 링크 |
|------|------|------------|------|
| MW1 | AI란 무엇인가 | LLM/할루시네이션/제조업 적용 개념 이해 | [[MW1-AI란-무엇인가]] |
| MW2 | AI 도구 체험 | ChatGPT·Claude·Gemini 비교 체험 | [[MW2-AI-도구-체험]] |
| MW3 | AI Studio 심화 | AI Studio 활용 심화 | [[MW3-AI-Studio-심화]] |
| MW4 | Claude Code 실습 | student-profile.md + 첫 Git push | [[MW4-Claude-Code-실습]] |
| MW5 | 워크플로우와 스킬 | CLAUDE.md + my-weekly-check.md + cae-judge 스킬 | [[MW5-워크플로우와-스킬]] |

### 주차 간 연결 흐름

```
[[MW1-AI란-무엇인가]]
  → [[MW2-AI-도구-체험]]
    → [[MW3-AI-Studio-심화]]
      → [[MW4-Claude-Code-실습]]  (설치 + student-profile)
        → [[MW5-워크플로우와-스킬]]  (CLAUDE.md + 스킬)
          → MW6 (vault + 통합 지식 워크플로우)  ← 지금 여기
```

---

## 핵심 개념 노드

| 개념 | 한 줄 요약 | 링크 |
|------|----------|------|
| CLAUDE.md | AI에게 주는 업무 규칙서. Always/Ask/Never 구조 | [[Concept-CLAUDE-md]] |
| student-profile.md | 내 역할·부서·업무를 AI가 기억하게 하는 파일 | [[Concept-student-profile]] |
| 프롬프트 | AI에게 잘 말하는 기술. 역할+맥락+형식+제약 4법칙 | [[Concept-프롬프트]] |
| 워크플로우 | 반복 가능한 작업 순서. 매주 같은 분석 자동화 | [[Concept-워크플로우]] |
| Git과 GitHub | 파일 변경 이력 관리. 저장해줘→올려줘 루프 | [[Concept-Git과-GitHub]] |
| 플러그인 | Claude Code 기능 확장 도구 | [[Concept-플러그인]] |

---

## A-RnD 업무 연결 노드

윤용식님 업무(CAE 구조해석 / 배터리 분석)와 연결되는 핵심 개념:

- **[[Concept-CLAUDE-md]]** → 응력>500MPa/안전계수<2.0 판정 규칙을 AI에게 저장
- **[[Concept-프롬프트]]** → CAE 결과를 팀 공유용 보고서로 바꾸는 4법칙 적용
- **[[Concept-워크플로우]]** → 매주 반복하는 CAE 결과 정리 자동화
- **[[MY-VAULT-NAVIGATION]]** → Mine/Reports에 보고서, Library/Specs에 스펙 문서 분리

---

## 바이브 코딩 — 핵심 발견 노드

> "코딩을 몰라도 AI에게 정확히 설명할 수만 있으면 개발자와 같은 결과물을 만든다"
> — [[MW5-워크플로우와-스킬]] 바이브 코딩 섹션

| 기존 방식 | 바이브 코딩 적용 |
|----------|---------------|
| 엑셀 함수 직접 작성 | "CAE 결과에서 응력 상위 5개 찾아서 표로 정리해줘" |
| Python 코드 작성 | "배터리 로그에서 이상 패턴 찾아줘" |
| 보고서 양식 수동 작성 | "팀장님 보고용 문체로 바꿔줘" |

---

## wikilink 그래프 허브 노드

아래 노트들이 가장 많이 참조됩니다 (그래프 뷰에서 큰 점으로 보임):

1. **[[Concept-프롬프트]]** — MW1/MW4/MW5에서 모두 참조
2. **[[Concept-CLAUDE-md]]** — MW5 Practice 06~09의 핵심
3. **[[Concept-워크플로우]]** — MW5 전체를 관통하는 개념
4. **[[MW5-워크플로우와-스킬]]** — 4개 concept이 연결된 허브

---

## 외부 자료 아카이브

| 출처 | 내용 | 저장 경로 |
|------|------|----------|
| (아직 없음 — Step 4 실습 후 채워짐) | | External-Research/ |

---

## AI 검색 가이드

이 vault에서 AI에게 물어볼 때:

```
"INDEX.md 참고해서 CAE 관련 내용 찾아줘"
"mw1-5-wiki에서 CLAUDE.md가 뭔지 설명해줘"
"내 Mine/Reports에서 응력 분석 보고서 찾아줘"
"Library/Specs에서 배터리 스펙 문서 있나 확인해줘"
```

---

*생성: 2026-04-16 | km-lite 자동 정리 | 출처: mw1-5-wiki/ (12개 파일)*
