# CLAUDE.md — 26-1 학기 인터랙티브 노트 프로젝트 지침서

> Claude Code가 이 프로젝트에서 작업할 때 반드시 먼저 읽고 따라야 하는 규칙입니다.

---

## 프로젝트 개요

- **소유자**: 민재 (성균관대학교 26-1)
- **목적**: 강의 노트를 과목별 인터랙티브 HTML로 변환하여 GitHub Pages로 배포

---

## 입력 방식 (중요!)

### 노션 API를 사용하지 않는다

- 콘텐츠는 사용자가 **파일로 직접 전달**한다
- 노션 MCP 호출 금지 — 토큰 낭비 방지

### 지원 파일 형식

| 형식 | 확장자 | 처리 방법 |
|---|---|---|
| 마크다운 | `.md` | 바로 텍스트 읽기 |
| 텍스트 | `.txt` | 바로 텍스트 읽기 |
| PDF | `.pdf` | 텍스트 추출 후 읽기 |
| Word | `.docx` | 텍스트 추출 후 읽기 |
| HTML | `.html` | 바로 텍스트 읽기 |

어떤 형식이든 내용만 추출해서 인터랙티브 HTML로 변환하면 된다. 파일 형식에 상관없이 동일한 품질의 결과물을 만들 것.

### 작업 흐름

```
사용자: [파일 첨부] + "3주차 시스템프로그램 정리해줘"
     ↓
Claude Code:
  1. 첨부된 파일 읽기 (pdf/docx면 텍스트 추출)
  2. 과목/주차 파악 (파일명 또는 사용자 메시지에서)
  3. 해당 과목 폴더에 HTML 파일 생성
  4. index.html의 weeks 배열 업데이트
  5. git add → commit → push
```

### 파일명에서 과목/주차 자동 감지

| 파일명 패턴 | 과목 | 주차 |
|---|---|---|
| `SP_Week3_*.*` | System Program | 3주차 |
| `Algo_Week2_*.*` | Algorithms | 2주차 |
| `OSS_Week1_*.*` | Open Source SW | 1주차 |
| `Logic_Week4_*.*` | 논리적사고 | 4주차 |
| `Myth_Week2_*.*` | 그리스로마신화 | 2주차 |
| `Life_Week3_*.*` | 생명의과학 | 3주차 |
| `Analects_Week1_*.*` | 성균논어 | 1주차 |

파일명으로 판별이 안 되면 사용자 메시지에서 과목과 주차를 파악한다.
확장자는 `.md`, `.txt`, `.pdf`, `.docx`, `.html` 모두 동일하게 처리.

---

## 폴더 구조

노션 26-1 과목 구조를 바탕화면에 미러링. 각 과목 폴더가 독립적인 Git 레포.

```
C:\Users\womin\OneDrive\바탕 화면\26-1\
├── system_program/           ← thdalswo4524/system_program
│   ├── CLAUDE.md
│   ├── index.html
│   ├── system_program_1week.html
│   ├── system_program_2week.html
│   └── system_program_3week.html
│
├── algorithms/               ← thdalswo4524/algorithms
│   ├── CLAUDE.md
│   ├── index.html
│   └── algorithms_1week.html
│
├── open_source_sw/           ← thdalswo4524/open_source_sw
├── logic/                    ← thdalswo4524/logic
├── greek_myth/               ← thdalswo4524/greek_myth
├── life_science/             ← thdalswo4524/life_science
└── sungkyun_analects/        ← thdalswo4524/sungkyun_analects
```

---

## 과목 ↔ 폴더 매핑

| 과목 | 폴더명 | 파일 prefix | 레포명 | accent 색상 |
|---|---|---|---|---|
| System Program | `system_program` | `system_program_` | `system_program` | cyan `#00e5ff` |
| Algorithms | `algorithms` | `algorithms_` | `algorithms` | green `#22c55e` |
| Open Source SW Practice | `open_source_sw` | `open_source_sw_` | `open_source_sw` | blue `#3b82f6` |
| 논리적사고 | `logic` | `logic_` | `logic` | orange `#f97316` |
| 그리스로마신화의이해 | `greek_myth` | `greek_myth_` | `greek_myth` | gold `#eab308` |
| 생명의과학 | `life_science` | `life_science_` | `life_science` | emerald `#10b981` |
| 성균논어 | `sungkyun_analects` | `sungkyun_analects_` | `sungkyun_analects` | rose `#f43f5e` |

---

## 규칙 1: HTML 파일 생성

### 파일명

`{과목prefix}_{N}week.html`

예: `system_program_4week.html`, `algorithms_3week.html`

### 디자인 규칙

- **단일 파일**: HTML, CSS, JS 모두 하나의 .html 파일에 포함
- **폰트**: `JetBrains Mono` (코드) + `Noto Sans KR` (본문) — Google Fonts CDN
- **테마**: 다크 배경 (`#0a~#0c` 계열)
- 과목별 메인 accent 색상 유지 (위 표 참조)
- 같은 과목 내에서도 **주차마다 서브 accent를 다르게**
- **구조**: Hero (Week N + 주제) → sticky Nav bar → 섹션별 콘텐츠
- **인터랙티브 요소**: 각 주요 개념마다 시뮬레이터/변환기/시각화 포함
- **퀴즈**: 마지막 섹션에 4~6문제, 정답/오답 하이라이트 + 해설
- **반응형**: 모바일 대응 (clamp, media query)
- **외부 라이브러리 금지**: Google Fonts만 허용, 나머지 vanilla JS/CSS

### 콘텐츠 변환 원칙

- 전달받은 파일 내용을 **그대로 복사하지 말고 인터랙티브하게 재구성**
- 표 → 인터랙티브 테이블 또는 시각화
- 공식 → 계산기/시뮬레이터
- 예제 → 단계별 애니메이션
- 개념 설명 → 탭/토글로 정리

---

## 규칙 2: index.html 업데이트

새 주차 파일 추가 시 해당 과목 폴더의 `index.html` 내 `weeks` 배열에 추가:

```javascript
{ week: N, title: "주제", desc: "키워드1 · 키워드2", file: "{prefix}_Nweek.html" },
```

### 새 과목 최초 생성 시

1. 과목 폴더 생성
2. 기존 `system_program/index.html`을 템플릿으로 `index.html` 생성
   - `<h1>` 제목을 과목명으로 변경
   - accent 색상 과목별로 조정
3. CLAUDE.md 복사 (이 파일)
4. Git 레포 초기 세팅

---

## 규칙 3: Git 워크플로우

### 기존 과목에 주차 추가

```bash
git add .
git commit -m "add week N: 주제명"
git push origin main
```

### 새 과목 레포 최초 세팅

```bash
cd "C:\Users\womin\OneDrive\바탕 화면\26-1\{폴더명}"
git init
git add .
git commit -m "init: {과목명}"
git branch -M main
git remote add origin git@github.com:thdalswo4524/{레포명}.git
git push -u origin main
```

GitHub에서 레포 생성 후 Settings → Pages → main branch 활성화.

---

## 규칙 4: 과목별 특수 규칙

### CS 과목 (System Program / Algorithms / Open Source SW)
- 코드 블록, 시뮬레이터, 알고리즘 시각화 중심
- 시간복잡도/공간복잡도 표기 포함

### 논리적사고
- 논증 구조 다이어그램, 벤 다이어그램, 진리표 인터랙티브
- 삼단논법 연습기

### 그리스로마신화의이해
- 타임라인, 관계도(가계도), 지도 시각화
- 신화 인물 카드

### 생명의과학
- 생물학 다이어그램, 프로세스 시각화
- 세포/유전자 관련 인터랙티브

### 성균논어
- 원문-해석 토글, 핵심 구절 하이라이트
- 주제별 분류

---

## 규칙 5: 코드 품질

- console 에러 없이 동작할 것
- 모든 인터랙티브 요소는 페이지 로드 시 초기값으로 자동 실행
- 한국어 UI, 기술 용어는 영어 유지 (과목 특성에 따라 유연하게)

---

## 규칙 6: 추가 지침

> 아래에 새로운 지침을 자유롭게 추가할 수 있습니다.

<!--
예시:
- 새 지침: OOO할 때는 XXX 방식으로 처리할 것
- 새 과목 추가: 매핑 테이블에 행 추가
-->

---

## 실행 예시

### 사용자: [SP_Week4_MachineProg.md 첨부] + "4주차 정리해줘"
1. 파일 읽기 (.md → 바로 읽기)
2. 과목: System Program, 주차: 4 감지
3. `system_program/system_program_4week.html` 생성
4. `system_program/index.html` weeks 배열에 추가
5. `git add . && git commit -m "add week 4: Machine-Level Programming" && git push origin main`

### 사용자: [lecture_note.pdf 첨부] + "알고리즘 5주차 정리해줘"
1. 파일 읽기 (.pdf → 텍스트 추출)
2. 파일명에서 감지 실패 → 메시지에서 "알고리즘 5주차" 파악
3. `algorithms/algorithms_5week.html` 생성
4. index.html 업데이트
5. git push

### 사용자: [week3_논리.docx 첨부] + "정리해줘"
1. 파일 읽기 (.docx → 텍스트 추출)
2. 파일명에서 "논리" + "week3" 감지 → 논리적사고 3주차
3. `logic/logic_3week.html` 생성
4. 폴더/레포 없으면 최초 세팅
5. git push
