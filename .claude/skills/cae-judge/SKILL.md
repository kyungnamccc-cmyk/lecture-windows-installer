---
name: cae-judge
description: CAE 구조해석 결과 자동 판정 스킬. result.csv를 읽어 응력/안전계수 기준으로 합격·불합격을 판정하고 판정결과.md + PDF를 자동 생성한다.
version: "1.0.0"
author: 윤용식 (성우하이텍 A-RnD)
allowed-tools: Read, Write, Bash, Glob
---

# CAE 판정 스킬

성우하이텍 A-RnD 구조해석 결과 자동 판정 루프.
`result.csv`를 읽어 판정 기준을 적용하고, 결과 파일과 PDF를 자동 생성합니다.

## 판정 기준

- 응력 > 500 MPa → **불합격**
- 안전계수 < 2.0 → **불합격**
- 두 조건 중 하나라도 해당하면 불합격

## 실행 순서

### Step 1 — 데이터 파일 확인

```
Read("practice-data/A-RnD/result.csv")
```

- 파일이 없으면 경로를 확인하고 중단
- 컬럼 확인: 부품번호, 최대응력(MPa), 안전계수

### Step 2 — 판정 실행 + 판정결과.md 저장

아래 프롬프트를 실행합니다:

```
@practice-data/A-RnD/result.csv 를 분석해서
응력 500MPa 초과 또는 안전계수 2.0 미만 부품을
합격/불합격 판정표로 만들어 practice-data/A-RnD/판정결과.md에 저장해줘.
표에는 부품번호, 응력(MPa), 안전계수, 판정, 불합격 사유를 포함해줘.
```

저장 위치: `practice-data/A-RnD/판정결과.md`

### Step 3 — 실무형 체크리스트 문서 생성 (v2)

```
@practice-data/A-RnD/판정결과.md 를 기반으로
응력 500MPa·안전계수 2.0 기준이 명시된 실무용 검토 체크리스트 문서로 바꿔서
practice-data/A-RnD/판정결과_v2.md로 저장해줘.
문서번호, 작성일, 불합격 원인별 분류, 담당자 서명란을 포함해줘.
```

저장 위치: `practice-data/A-RnD/판정결과_v2.md`

### Step 4 — Typst PDF 생성

아래 Bash 명령으로 PDF를 생성합니다:

```bash
# Typst 경로 (Windows winget 설치 기준)
TYPST="C:/Users/enter/AppData/Local/Microsoft/WinGet/Packages/Typst.Typst_Microsoft.Winget.Source_8wekyb3d8bbwe/typst-x86_64-pc-windows-msvc/typst.exe"

# .typ 파일 생성 후 컴파일
"$TYPST" compile \
  "practice-data/A-RnD/판정결과_v2.typ" \
  "practice-data/A-RnD/판정결과_v2.pdf"
```

> ⚠️ `.typ` 파일이 없으면 Step 3 결과를 Typst 형식으로 먼저 변환해야 합니다.

### Step 5 — 결과 저장 + GitHub 업로드

```
저장해줘
올려줘
```

## 산출물

| 파일 | 설명 |
|------|------|
| `판정결과.md` | 기본 합격/불합격 판정표 |
| `판정결과_v2.md` | 실무형 체크리스트 (기준값 명시 + 서명란) |
| `판정결과_v2.pdf` | 인쇄/보고용 PDF |

## 활용 시나리오

- 매주 새 CAE 결과가 나올 때 → `result.csv` 교체 후 스킬 실행
- 판정 기준이 바뀔 때 → 프롬프트의 기준값만 수정 후 재실행
- 2개 이상 데이터 교차 분석 → Practice 08의 `/using-superpowers` 루프와 결합

## 트러블슈팅

| 문제 | 해결 |
|------|------|
| `result.csv` 없음 | 파일 경로 확인, `@` 파일 지정 방식으로 재시도 |
| 판정 기준 다를 때 | 프롬프트에 새 기준값 명시 후 재실행 |
| Typst PDF 생성 실패 | Typst 경로 확인, `typst.exe` 위치 재확인 |
| 판정 결과가 이상함 | 기준값을 프롬프트 안에 다시 직접 명시 |
