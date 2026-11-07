# 디자인 시트 — 다크 · 라이트 에디터

다크·라이트 두 테마의 색상 토큰과 컴포넌트 규칙을 정리한 디자인 시트다. 새 에디터(코드 편집기·설정 화면·외부 도구)를 같은 모양으로 만들 때 이 문서의 토큰과 규칙만 따르면 된다.

색 토큰은 JetBrains IDE 의 New UI 다크·라이트 색을 바탕으로 하고, 코드 문법 색은 JetBrains IntelliJ 의 다크/라이트 스킴을 따른다.
---

## 1. UI 컨셉

### 1.1 한 줄 정의

> **"IDE 안에 들어 있는 도구 창."** 장식이 아니라 밀도로 승부하는 JetBrains New UI 계열의 평면(flat) 데스크톱 UI 에 절제된 무채색 조작 방식을 얹었다. 바탕은 무채색 3~4단계로만 깊이를 만들고, 색은 **의미가 있을 때만**(강조·성공·경고·위험·파일 상태·diff) 쓴다.

### 1.2 컨셉 키워드

| 키워드 | 뜻 | 화면에서 드러나는 방식 |
|---|---|---|
| **평면 · 무그림자** | 입체감 대신 바탕 밝기 차이로 층을 나눈다 | 패널·버튼·입력칸에 그림자 없음. 그림자는 떠 있는 것(메뉴·모달·토스트·드로어·HUD)에만 |
| **밀도** | 13px 본문, 20px 파일 줄, 28px 메뉴 줄, 32px 머리 줄 | 여백은 4·6·8·10·12px 단위로 좁게. 글자 크기는 9~15px 사이에서만 움직인다 |
| **무채색 조작** | 자주 쓰는 Stage/Unstage 는 무채색 외곽선 버튼. 색은 되돌릴 수 없는 것(버리기·충돌)에만 | 머리 줄 버튼은 `field-border` 외곽선 + 투명 바탕, hover 는 회색 한 단계 |
| **무채색 바탕 + 의미색 점** | 회색 바탕 위에 파랑(강조)·초록(성공/현재)·호박(경고)·빨강(위험)만 찍는다 | 의미색 버튼은 "옅은 바탕(soft) + 같은 색 테두리 → 올리면 꽉 찬 색(strong)" |
| **선 1px** | 구분은 1px 실선 하나 | 패널 경계·머리 줄 아래·메뉴 구분선 모두 `--git-border`/`--git-divider` 1px |
| **둥글기 6 · 4** | 컨트롤 6px, 작은 컨트롤 4px | 떠 있는 상자는 8~12px, 경고 카드 14px, HUD 24px |
| **텍스트 위계 4단** | strong > text > secondary > muted | 제목/현재 항목은 strong, 본문은 text, 구역 제목은 secondary, 보조·캡션·꺼진 것은 muted |
| **JetBrains 파랑** | 강조는 한 색(#3871e1 / #3574f0) | 포커스 링, 선택 줄, primary 버튼, 진행 막대, 뱃지, 링크, diff hover 띠 |
| **한국어 친화 글꼴** | IBM Plex Sans KR 하나로 UI 전체, 코드는 JetBrains Mono | 굵기 400·500·600·700 만 쓴다 |

### 1.3 다크와 라이트의 성격 차이

| | 다크 | 라이트 |
|---|---|---|
| 바탕 | 거의 검정 `#191a1c`. 사이드 패널은 한 단계 밝게 `#26282c` 로 띄운다 | 순백 `#ffffff`. 사이드·도구 막대·대화상자는 살짝 회색 `#f7f8fa` 로 가라앉힌다 |
| 층 나누기 | **밝아질수록 위** (bg → sidebar → hover → pressed) | **어두워질수록 위** (bg → sidebar → hover → pressed) |
| 글자 | 회백 `#d1d3d9`, 강조만 순백 | 순검정 `#000000`, 보조는 청회색 |
| 강조 파랑 | `#3871e1`, 글자용은 더 밝은 `#7aa7ff` | `#3574f0`, 글자용은 더 진한 `#1d55b8` |
| 의미색 | 채도 낮춘 파스텔(`#5fb865` `#d6ae58` `#db5c5c`) | 채도·명도 낮춘 진한 색(`#208a3c` `#a46704` `#db3b4b`) |
| 반투명(soft) | 22~28% | 12% |
| 그림자 | 진하게 0.28~0.45 | 옅게 0.06~0.14 |
| 선택 줄 | 남색 `#2a4371` 위에 밝은 글자 | 연파랑 `#d5e1ff` 위에 검정 글자 |

---

## 2. 타이포그래피

| 토큰 | 값 |
|---|---|
| `--font` | `"IBM Plex Sans KR", -apple-system, BlinkMacSystemFont, "Segoe UI", "Malgun Gothic", "Apple SD Gothic Neo", sans-serif` |
| `--mono` | `"JetBrains Mono", "SF Mono", Menlo, Consolas, monospace` |
| 본문 | `--base-size`(기본 13px) / 줄높이 1.4, `-webkit-font-smoothing: antialiased` |
| 굵기 | 400 본문 · 500 보조 숫자·뱃지 · 600 제목·구역·현재 항목 · 700 강조 제목·머리 줄·현재 브랜치 |

**크기 사다리**

| 크기 | 쓰는 곳 |
|---|---|
| 9px | 도구 막대 캡션(`.tb-caption`), 도구 막대 뱃지 |
| 10px | 탐색기 머리 줄, 도구 막대 버튼 글자, 사이드바 탭 뱃지, 푸터 버전 |
| 10.5px | 최근 목록 뱃지 |
| 11px | diff 통계, 사이드바 수, 푸터 글자, 커밋 칸 라벨, 참조 이름표 |
| 11.5px | `.small`, 작은 버튼, 메뉴 힌트, 파일 경로, 구역 개수 |
| 12px | 모노 코드, diff 본문, 입력칸·체크박스, 구역 제목, 모달 라벨, 탐색기 칸 글자 |
| 12.5px | 사이드바 필터 입력 |
| **13px** | **본문 기본**, 메뉴 항목, 탭, 커밋 제목, 토스트, 경고 카드 제목 |
| 14px | 커밋 칸 작성자 이름 |
| 15px | 모달 제목, 커밋 제목(상세), 설정 구역 제목 |
| 16px | 드로어 제목 |
| 19px | 전환 HUD 이름 |
| 28px | 시작 화면 h1 |

UI 배율은 `--base-size` 하나를 바꿔 전체를 키운다(⌘+ / ⌘− / ⌘0, 설정 `ui.fontSize`).

---

## 3. 치수 · 둥글기 토큰

| 토큰 | 값 | 쓰는 곳 |
|---|---|---|
| `--radius` / `--git-radius` | 6px | 버튼, 입력칸, 도구 막대 버튼, 메뉴 항목 |
| `--radius-small` / `--git-radius-small` | 4px | 작은 버튼, 뱃지, 툴팁, 참조 이름표 |
| `--bar-h` | 32px | 패널 머리 줄(도구 막대·diff 막대·스테이징 머리·탐색기 막대·커밋 칸 탭 줄) 높이 |
| `--bar-pad` | 10px | 머리 줄 좌우 안쪽 여백 |
| `--bar-gap` | 8px | 머리 줄 항목 사이 |
| `--ctl-h` | 22px | 머리 줄 안 컨트롤 높이 |

**고정 높이**

| 요소 | 높이 |
|---|---|
| 탭 막대 | 38px (탭 32px) |
| 도구 막대 | 48px (버튼 52×42) |
| 패널 머리 줄 | 32px |
| 탐색기 머리 줄 | 24px |
| 탐색기 커밋 줄 | 34px |
| 파일 줄 | 20px (탐색기 커밋 칸 파일 24px) |
| 사이드바 탭·구역·항목 | 28px |
| 메뉴 항목 | 28px |
| 버튼 | 28px (small 22, tiny 19, wide 32, sheet 30, 경고 카드 26) |
| 푸터 | 24px |
| 스크롤바 | 10px (thumb 2px 투명 테두리로 6px 로 보임) |

**둥글기 사다리**: 3px(체크) → 4px(작은 것·이름표) → 5px(목록 안 둥근 줄) → 6px(컨트롤) → 7px(경고 카드 버튼) → 8px(팝업 목록, 탭 윗모서리) → 10px(메뉴·모달·토스트) → 12px(시트) → 14px(경고 카드) → 24px(HUD) → 999px(진행 막대·원형 뱃지).

---

## 4. 색상 팔레트

모든 색은 `:root[data-theme="dark"]` / `:root[data-theme="light"]` 의 `--git-*` 변수다. 테마는 `<html data-theme>` 으로 바뀌고 `system` 은 `prefers-color-scheme` 을 따른다.

### 4.1 바탕(Surface)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-bg` | `#191a1c` | `#ffffff` | 창 기본 바탕, 가운데(목록·diff) 바탕 |
| `--git-sidebar-bg` | `#191a1c` → 패널 안 `#26282c` | `#f7f8fa` | 왼쪽 사이드바, 커밋 칸, 탐색기 아래 칸 |
| `--git-toolbar-bg` | `#191a1c` | `#f7f8fa` | 도구 막대, 활성 탭 |
| `--git-header-bg` | `#191a1c` → 패널 안 `#26282c` | `#f7f8fa` | 패널 머리 줄, 커밋 상자, 푸터, 탐색기 막대 |
| `--git-list-bg` | `#191a1c` → 패널 안 `#26282c` | `#ffffff` | 목록 바탕 |
| `--git-titlebar-bg` | `#0f1012` | `#e4e6ea` | 탭 막대(가장 아래 층) |
| `--git-titlebar-hover` | `#1d1e21` | `#eceef1` | 탭 hover |
| `--git-dialog-bg` | `#191a1c` | `#f7f8fa` | 시트형 대화상자 바탕 |
| `--git-menu-bg` | `#2b2d30` | `#ffffff` | 메뉴·팝업·모달·토스트 바탕 |
| `--git-combo-bg` | `#25282c` | `#ffffff` | 콤보 상자 |
| `--git-field-bg` | `#191a1c` → 패널 안 `#26282c` | `#ffffff` | 입력칸, 커밋 상세 카드 |
| `--git-button-bg` | `#191a1c` → 패널 안 `#26282c` | `#ffffff` | 기본 버튼 |
| `--git-badge-bg` | `#33353b` | `#ebecf0` | 무채색 뱃지, 사이드바 탭 뱃지 |
| `--git-connect-card` | `#212226` | `#ffffff` | 연결 화면 카드 |
| `--git-graph-lane-bg` | `#26282c` | `#f2f3f5` | 탐색기 레인 바탕 |

> **다크 전용 규칙**: `.sidebar`·`.detail` 안에서는 sidebar/header/list/field/button 바탕이 `#26282c` 로 한 단계 올라가고, hover `#33353b` · pressed `#3c3f45` · divider `#33353b` 도 함께 한 단계씩 올라간다. 라이트는 나누지 않는다.

**다크 바탕 사다리**: `#0f1012`(탭 막대) → `#191a1c`(창) → `#202124`(diff 빈 칸) → `#212226`(카드) → `#25282c`(콤보) → `#26282c`(패널·hover) → `#2b2d30`(메뉴) → `#303238`(현재 줄) → `#33353b`(pressed·뱃지) → `#3c3f45`(패널 pressed) → `#40434a`(입력 테두리) → `#43454a`(메뉴 테두리)

**라이트 바탕 사다리**: `#ffffff`(창) → `#f7f8fa`(패널) → `#f2f3f5`(레인) → `#f0f1f3`(버튼 hover) → `#ebecf0`(선·뱃지) → `#e9ebef`(현재 줄) → `#e4e6ea`(탭 막대) → `#dfe1e5`(hover·메뉴 테두리) → `#d3d5db`(pressed) → `#c9ccd6`(입력 테두리)

### 4.2 글자(Text)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-text-strong` | `#ffffff` | `#000000` | 제목, 현재 브랜치, 활성 탭, 모달 제목 |
| `--git-text` | `#d1d3d9` | `#000000` | 본문 |
| `--git-text-secondary` | `#a8abb2` | `#4e5260` | 구역 제목, 모달 본문, 폴더 이름, 요약 글 |
| `--git-text-muted` | `#80838a` | `#6c707e` | 캡션, 힌트, 아이콘 기본색, 꺼진 메뉴, 헝크 머리 |
| `--git-selected-text` | `#d1d3d9` | `#000000` | 선택 줄 위 글자 |
| `--git-menu-hover-text` | `#dfe1e5` | `#000000` | 메뉴 hover 글자 |
| `--git-accent-text` / `--git-on-strong` | `#ffffff` | `#ffffff` | 강조·strong 바탕 위 글자 |
| `--git-action-icon` | `#d1d3d9` | `#5e6270` | 도구 막대 아이콘 |
| `--git-graph-subject` | `#b0b3ba` | `#494b57` | 탐색기 커밋 제목 |
| `--git-graph-date` | `#b4b7bd` | `#6c707e` | 탐색기 날짜 |
| `--git-graph-empty` | `#6a6d74` | `#818594` | 빈 목록 안내 |
| `--git-diff-text` | `#bcbec4` | `#080808` | diff·코드 본문 |
| `--git-diff-number` | `#4c4f56` | `#a8adbd` | 줄 번호 |
| `--git-editor-number-current` | `#a1a3ab` | `#767a8a` | 현재 줄 번호 |
| `--git-editor-caret` | `#cecfd2` | `#000000` | 캐럿 |

### 4.3 선(Border · Divider)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-border` | `#33353b` | `#ebecf0` | 패널 경계, 머리 줄 아래, 분할선, 카드 테두리, 헝크 머리 아래 |
| `--git-divider` | `#26282c` → 패널 안 `#33353b` | `#ebecf0` | 목록 안 구분선, 도구 막대 묶음 사이 |
| `--git-dialog-border` | `#33353b` | `#dfe1e5` | 시트 테두리 |
| `--git-dialog-divider` | `#25282c` | `#ebecf0` | 시트 머리/바닥 구분, 작업 목록 줄 사이 |
| `--git-field-border` | `#40434a` | `#c9ccd6` | 입력칸·외곽선 버튼(Stage/Unstage) 테두리 |
| `--git-button-border` | `#40434a` | `#c9ccd6` | 기본 버튼 테두리 |
| `--git-menu-border` | `#43454a` | `#dfe1e5` | 메뉴·모달·토스트 테두리, 메뉴 구분선 |
| `--git-graph-header-border` | `#26282c` | `#ebecf0` | 탐색기 머리 줄 아래 |
| `--git-diff-split-border` | `#26282c` | `#ebecf0` | diff 양쪽 보기 가운데 선 |
| `--git-connect-card-border` | `rgba(255,255,255,0.08)` | `rgba(0,0,0,0.08)` | 카드 테두리 |

### 4.4 상호작용 상태(State)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-hover` | `#26282c` → 패널 안 `#33353b` | `#dfe1e5` | 줄·버튼 hover, Stage 버튼 hover |
| `--git-button-hover` | `#26282c` → 패널 안 `#33353b` | `#f0f1f3` | 기본 버튼 hover |
| `--git-pressed` | `#33353b` → 패널 안 `#3c3f45` | `#d3d5db` | 눌림, ⋮ 버튼 hover, 충돌 해결 버튼, diff 동작 버튼 |
| `--git-selected` | `#2a4371` | `#d5e1ff` | 선택 줄, 켜진 토글(.on), 사이드바 탭 |
| `--git-current-row` | `#303238` | `#e9ebef` | 포커스 잃은 선택 줄 |
| `--git-menu-hover` | `#2e436e` | `#d5e1ff` | 메뉴 항목 hover·활성, 팝업 목록 선택 |
| `--git-accent-soft` | `rgba(56,113,225,0.28)` | `rgba(53,116,240,0.12)` | 커밋 버튼 바탕 |
| `--git-diff-text-selection` | `rgba(33,66,131,0.95)` | `rgba(166,210,255,0.9)` | 코드 드래그 선택 |
| `--git-editor-current-line` | `rgba(255,255,255,0.045)` | `rgba(0,0,0,0.045)` | 편집기 현재 줄 |
| `--git-editor-indent-guide` | `rgba(255,255,255,0.09)` | `rgba(0,0,0,0.10)` | 들여쓰기 안내선 |
| 탐색기 선택 줄 | `rgba(59,130,196,0.3)` | 같음 | 고른 커밋 줄 바탕 |

### 4.5 강조(Accent)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-accent` | `#3871e1` | `#3574f0` | primary 버튼, 포커스 테두리·링, 진행 막대, 뱃지, 열 손잡이 hover, diff hover 띠 테두리 |
| `--git-accent-hover` | `#3366cc` | `#3369d6` | primary hover, 커밋 버튼 active |
| `--git-accent-fg` | `#7aa7ff` | `#1d55b8` | 글자용 파랑: 링크, 토스트 아이콘, 해시·파일 경로 hover, 작업 중 탭 아이콘 |
| `--git-section-count` | `#7aa7ff` | `#1d55b8` | 사이드바 구역 개수 |
| `--git-refs` | `#3876fd` | `#2e55a3` | 참조 색 |
| `--git-graph-selection` | `#d1d3d9` | `#3574f0` | 탐색기 선택 표시 |

### 4.6 의미색(Semantic) — 4값 세트

각 의미색은 **글자/테두리(base) · 옅은 바탕(soft) · 꽉 찬 바탕(strong) · 눌림(strong-pressed)** 네 값이 한 세트다.

| 세트 | 토큰 | 다크 | 라이트 |
|---|---|---|---|
| 성공 · 현재 브랜치 · 완료 | `--git-success` | `#5fb865` | `#208a3c` |
| | `--git-success-soft` | `rgba(95,184,101,0.24)` | `rgba(32,138,60,0.12)` |
| | `--git-success-strong` | `#5fb865` | `#208a3c` |
| | `--git-success-strong-pressed` | `#4f9d55` | `#1b7533` |
| 경고 · 병합 중 · 미해결 | `--git-warning` | `#d6ae58` | `#a46704` |
| | `--git-warning-soft` | `rgba(214,174,88,0.22)` | `rgba(164,103,4,0.12)` |
| | `--git-warning-strong` | `#d6ae58` | `#a46704` |
| | `--git-warning-strong-pressed` | `#bf9a4a` | `#8c5803` |
| 위험 · 버리기 · 실패 | `--git-danger` | `#db5c5c` | `#db3b4b` |
| | `--git-danger-soft` | `rgba(219,92,92,0.26)` | `rgba(219,59,75,0.12)` |
| | `--git-danger-strong` | `#db5c5c` | `#db3b4b` |
| | `--git-danger-strong-pressed` | `#c24c4c` | `#c22f3f` |
| strong 위 글자 | `--git-on-strong` | `#ffffff` | `#ffffff` |

보조 고정색: 경고 삼각 **아이콘**은 라이트에서 글자용 갈색 대신 `#f0a30a`(`--alert-icon`). 탐색기 검색 강조 `--git-graph-search` 다크 `#d6ae58` / 라이트 `#e5a823`.

### 4.7 파일 상태(File state)

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-state-modified` | `#70aeff` | `#3574f0` | 수정 |
| `--git-state-added` | `#73bd79` | `#208a3c` | 추가·추적 안 됨 아이콘 |
| `--git-state-deleted` | `#db5c5c` | `#db3b4b` | 삭제 |
| `--git-state-renamed` | `#6abec2` | `#078080` | 이름 변경 |
| `--git-state-untracked` | `#e88f89` | `#db3b4b` | 추적 안 됨 |
| `--git-state-conflicted` | `#f75464` | `#d81b60` | 충돌 글자 |
| `--git-state-text` | `#191a1c` | `#ffffff` | 상태 아이콘 위 글자 |
| `--git-count-added` | `#73bd79` | `#208a3c` | +n 수 |
| `--git-count-deleted` | `#e88f89` | `#db3b4b` | −n 수 |

파일 종류 아이콘(테마 공통): 이미지 `#b885ff` · 코드 `#599eff` · 데이터 `#f2cc4d` · 에셋 `#4dcccc` · 미디어 `#ff73a6` · 바이너리 `#ff9e40` · 텍스트 `--git-text-secondary`.

### 4.8 Diff

| 토큰 | 다크 | 라이트 |
|---|---|---|
| `--git-diff-bg` | `#191a1c` | `#ffffff` |
| `--git-diff-gutter` | `#191a1c` | `#ffffff` |
| `--git-diff-empty` | `#202124` | `#f7f8fa` |
| `--git-diff-added-line` | `#22352a` | `#eaf5ea` |
| `--git-diff-added-change` | `#33573c` | `#bee6c1` |
| `--git-diff-removed-line` | `#4a1d23` | `#ffe1e3` |
| `--git-diff-removed-change` | `#7d2630` | `#ffb8bf` |
| `--git-diff-hunk` | `#233042` | `#edf3ff` |
| `--git-diff-hunk-text` | `#6f96f0` | `#3574f0` |
| `--git-diff-added-sign` | `#73bd79` | `#208a3c` |
| `--git-diff-removed-sign` | `#ff5c6c` | `#db3b4b` |
| 덩어리 hover 띠 | `rgba(80,140,230,0.10)` + `--git-accent` 1px | 같음 |

> 헝크 머리(`@@ …`)는 `--git-diff-hunk` 바탕을 **칠하지 않고** `--git-text-muted` 흐린 글자 + 아래 1px `--git-border` 로만 나눈다. hunk 토큰은 다른 보기 모드용으로 남겨 둔다.

### 4.9 코드 문법(Syntax) — IntelliJ

| 토큰 | 다크 (IntelliJ Dark) | 라이트 (IntelliJ Light) |
|---|---|---|
| `--git-code-keyword` / `control` / `tag` | `#6c95eb` | `#0f54d6` |
| `--git-code-string` | `#c9a26d` | `#8c6c41` |
| `--git-code-number` | `#ed94c0` | `#a7336a` |
| `--git-code-comment` | `#85c46c` | `#288307` |
| `--git-code-key` / `attribute` | `#66c3cc` | `#0091a0` |
| `--git-code-type` | `#c191ff` | `#6c32b8` |
| `--git-code-function` | `#39cc9b` | `#00855f` |
| `--git-code-punctuation` / `bracket-*` | `var(--git-diff-text)` | `var(--git-diff-text)` |

| 파이썬 토큰 | 다크 (IntelliJ Dark) | 라이트 (IntelliJ Light) |
|---|---|---|
| `--git-code-py-keyword` / `self` / `escape` | `#cf8e6d` | `#0033b3` / `#94558d` / `#0037a6` |
| `--git-code-py-string` | `#6aab73` | `#067d17` |
| `--git-code-py-docstring` | `#5f826b` | `#8c8c8c` |
| `--git-code-py-number` | `#2aacb8` | `#1750eb` |
| `--git-code-py-comment` | `#7a7e85` | `#8c8c8c` |
| `--git-code-py-kwarg` | `#aa4926` | `#660099` |
| `--git-code-py-function` | `var(--git-diff-text)` | `#00627a` |
| `--git-code-py-decorator` | `#b3ae60` | `#9e880d` |

### 4.10 탐색기(커밋 목록)

| 토큰 | 다크 | 라이트 |
|---|---|---|
| `--git-graph-bg` / `header-bg` | `#191a1c` / `#191a1c` | `#ffffff` / `#f7f8fa` |
| `--git-graph-pill-bg` | `#233558` | `#3574f0` |
| `--git-graph-pill-head` | `#2f6b3a` | `#208a3c` |
| `--git-graph-pill-tag` | `#c79a3a` | `#a46704` |
| `--git-graph-pill-text` | `#ffffff` | `#ffffff` |
| `--git-graph-head` | `#5fb865` | `#208a3c` |
| `--git-graph-lane-text` | `#d1d3d9` | `#000000` |
| `--git-graph-date-band` | `rgba(255,255,255,0.06)` | `rgba(0,0,0,0.05)` |
| `--git-graph-truncated` | `rgba(128,131,138,0.5)` | `rgba(108,112,126,0.5)` |
| `--git-graph-tooltip-bg` / `text` / `border` | `#26282c` / `#d1d3d9` / `#43454a` | `#ffffff` / `#000000` / `#dfe1e5` |
| `--git-graph-tooltip-shadow` | `rgba(0,0,0,0.45)` | `rgba(0,0,0,0.14)` |

**가지 색(8색, 테마 공통)** — 브랜치 이름으로 번호를 정하므로 새 커밋이 생겨도 색이 바뀌지 않는다. 이름 없는 가지(지워진 브랜치)는 회색 `#808080` 과 반 섞는다.

```
#599eff  #59cc73  #ff9e40  #b885ff  #ff73a6  #4dcccc  #f2cc4d  #f26659
```

참조 이름표(`.chx-ref`): 둥글기 4, 11px 흰 글자, 바탕은 브랜치 `--git-graph-pill-bg` · 현재 `--git-graph-pill-head` · 태그 `--git-graph-pill-tag`.

병합 편집기 고정색: A(현재) 청록 `#2fa4c2`(띠 0.22/현재 0.32) · B(들어오는) 노랑 `#d9a732`(띠 0.24/0.34) · 미해결 `rgba(224,160,48,0.08)` + 경고색 글자. 병합 이름표: incoming `#2f5fa8`, target `#1f7a74`.

### 4.11 떠 있는 것(Overlay) · 그림자 · 반투명

| 토큰 | 다크 | 라이트 | 역할 |
|---|---|---|---|
| `--git-menu-shadow` | `rgba(0,0,0,0.45)` | `rgba(0,0,0,0.12)` | 메뉴 `0 10px 28px` + `0 1px 3px`, 모달 `0 12px 40px`, 팝업 `0 8px 24px`, 드로어 `8px 0 24px` |
| `--git-shadow-large` | `rgba(0,0,0,0.28)` | `rgba(0,0,0,0.06)` | 큰 카드 |
| `--git-shadow-small` | `rgba(0,0,0,0.24)` | `rgba(0,0,0,0.08)` | 작은 카드 |
| `--git-graph-shadow` | `rgba(0,0,0,0.35)` | `rgba(0,0,0,0.14)` | 탐색기 열 끌기 유령 |
| 모달 막 | `rgba(0,0,0,0.45)` | 같음 | `.modal-backdrop` |
| `--scrim-bg` | `rgba(0,0,0,0.32)` | `rgba(0,0,0,0.16)` | HUD 뒤 막 + blur 5px |
| `--git-scroll-thumb` / `hover` / `active` | 흰 0.18 / 0.30 / 0.42 | 검정 0.18 / 0.30 / 0.42 | 스크롤바 |
| `--git-skeleton` | `rgba(255,255,255,0.16)` | `rgba(0,0,0,0.13)` | 로딩 막대 |
| `--jobs-track` | `rgba(255,255,255,0.12)` | `rgba(0,0,0,0.08)` | 진행 막대 바닥 |
| `--git-connect-card-highlight` | `rgba(255,255,255,0.18)` | `rgba(0,0,0,0.2)` | 카드 하이라이트 |
| `--git-connect-code-cell` | `rgba(255,255,255,0.05)` | `rgba(0,0,0,0.03)` | 코드 셀 |

**전환 HUD(macOS 반투명 창)**

| 토큰 | 다크 | 라이트 |
|---|---|---|
| `--hud-bg` | `rgba(38,39,43,0.62)` | `rgba(250,250,252,0.66)` |
| `--hud-border` | `rgba(255,255,255,0.10)` | `rgba(0,0,0,0.08)` |
| `--hud-highlight` | `rgba(255,255,255,0.08)` | `rgba(255,255,255,0.7)` |
| `--hud-text` / `--hud-sub` | `rgba(255,255,255,0.94)` / `rgba(235,235,245,0.55)` | `rgba(0,0,0,0.88)` / `rgba(60,60,67,0.6)` |
| `--hud-blade` | `#f2f2f7` | `#3c3c43` |
| `--tab-shine` | `rgba(255,255,255,0.95)` | `rgba(120,160,255,1)` |
| blur | `blur(28px) saturate(180%)`, 둥글기 24px, 0.5px 테두리 | 같음 |

**툴팁(`.callout`)**: 테마와 상관없이 검정 `#000` 바탕, 흰 12px 글자, 둥글기 4, 꼭지 6px, 그림자 `0 2px 6px rgba(0,0,0,0.3)`.

---

## 5. 컴포넌트 스타일

### 5.1 바디 · 창 틀

```css
html, body { background: var(--git-bg); color: var(--git-text);
  font: var(--base-size, 13px)/1.4 var(--font); -webkit-font-smoothing: antialiased; user-select: none; }
```

- 글은 기본으로 **드래그 선택 불가**. 입력칸·코드·메시지·토스트·커밋 정보 칸만 `user-select: text`.
- 창 배치: 탭 막대(38) → 도구 막대(48) → [사이드바 240 | 1px | **가운데 Unstaged(위)/Staged(아래) 목록** | 1px | **오른쪽 위 diff · 아래 커밋 칸**] → 푸터(24).
  - 가운데 목록 머리 줄 32px, Staged 높이는 경계를 끌어 바꾼다(처음 200px).
  - 오른쪽 커밋 칸 `.fork-commit` 은 위 1px `--git-border` + `--git-sidebar-bg` 로 diff 와 나뉜다.
  - 병합 편집기를 열면 목록·커밋 칸을 접고 편집기가 가득 찬다.
- 탐색기(히스토리) 탭: 위 커밋 목록 [그래프 · 메시지 · 작성자 150 · SHA 84 · 날짜 150], 아래 칸 [Commit · Changes · File Tree] 단추와 고른 칸.
- 패널 사이 분할선은 1px `--git-border` 이고 잡는 영역은 좌우 3px 씩 넓힌다. 끌 때 커서 `col-resize`.
- 스크롤바: 10px, 트랙 투명, thumb 는 2px 투명 테두리 + `background-clip: content-box` 로 가늘게.

### 5.2 버튼

**기본 `.btn`** — 높이 28, 좌우 12px, 1px `--git-button-border`, 둥글기 6, 바탕 `--git-button-bg`, 글자 상속.

| 변형 | 바탕 | 테두리 | 글자 | hover | 비고 |
|---|---|---|---|---|---|
| `.btn` | button-bg | button-border | text | button-hover | |
| `.btn.primary` | accent | accent | `#fff` | accent-hover | 한 창에 하나 |
| `.btn.on` | selected | | | | 켜진 토글 |
| `.btn.success` | success-soft | success-soft | success | **success-strong + on-strong** | 완료·확인 |
| `.btn.danger` | danger-soft | danger-soft | danger | **danger-strong + on-strong** | 버리기 |
| `.btn.danger-strong` | danger | danger | `#fff` | | 되돌릴 수 없는 확인 |
| `.btn.warning` | warning-soft | warning | warning | | 충돌 해결 |
| `.btn.small` | | | 11.5px | | 높이 22, 좌우 8, 둥글기 4 |
| `.btn.tiny` | | | 11px | | 높이 19, 좌우 7, 둥글기 4 |
| `.btn.wide` | | | | | 폭 100%, 높이 32 |
| `.link` | 없음 | 없음 | accent-fg | | 텍스트 링크 |
| `:disabled` | | | | | `opacity: 0.45`, 커서 기본 |

**머리 줄 무채색 버튼(`.staging-btn.fork-head`)** — Stage / Unstage(Shift 를 누르는 동안 "… All"). 색을 쓰지 않는다.

```
기본   : 높이 22 · 좌우 10 · 1px --git-field-border · 투명 바탕 · 글자 text · 12px
hover  : 바탕 --git-hover · 테두리 그대로
active : 바탕 --git-pressed
disabled(고른 것 없음) : opacity 0.5
충돌 해결(.resolve) : 테두리 --git-button-border · 바탕 --git-pressed (역시 무채색)
```

**의미색 버튼의 3단 규칙**(헝크 `.hunk-btn`, 모든 변경 버리기 `.wip-discard`, `.btn.success/.danger`):

```
기본   : 바탕 <색>-soft   · 테두리 <색>        · 글자 text
hover  : 바탕 <색>-strong · 테두리 <색>-strong · 글자 on-strong(#fff)
active : 바탕 <색>-strong-pressed · 테두리 같음
```

**아이콘 버튼(도구 막대 `.tb-btn`)** — 52×42, 테두리 없음, 투명 바탕, 아이콘 20px `--git-action-icon` 위에 10px 캡션 `--git-text-muted`. hover `--git-hover`, active `--git-pressed`, disabled `opacity 0.35`. 작업 중(`.running`)은 흐려지지 않고 아이콘 자리에 2px 링 스피너(`--git-border` + `--git-accent` 윗부분)가 돈다. 뱃지는 아이콘 오른쪽 위 13px 알약(`--git-accent`, 9px 700).

**머리 줄 토글(`.dbar-btn`, `.view-switch`, `.chx-tab`)** — 높이 18~24, 투명, 글자 muted, hover `--git-hover` + text, 켜짐(`.on`)은 `--git-selected` 바탕 + `--git-selected-text`. 묶음(`.view-switch`)은 1px `--git-field-border` 로 감싸고 사이도 1px.

**둥근 작은 버튼(`.tab-folder`, `.tab-new`, `.tab-jobs`, `.side-more`, `.recent-more`, `.chx-github`)** — 20~28px 정사각, 둥글기 4~6, 투명, 글자 `--git-text-muted`, hover 는 `--git-hover`(또는 `--git-pressed`) + `--git-text`.

**커밋 버튼(`.commit-btn`)** — 높이 26, 테두리 `--git-accent`, 바탕 `--git-accent-soft`; hover 는 `--git-accent` 꽉 채움 + 흰 글자; active `--git-accent-hover`; disabled 는 기본 버튼 모양으로 돌아가고 글자 muted. 옆 Push 버튼은 기본 `.btn` 모양.

### 5.3 입력칸

```css
.field { padding: 6px 8px; border: 1px solid var(--git-field-border); border-radius: var(--radius);
  background: var(--git-field-bg); color: var(--git-text); outline: none; }
.field:focus { border-color: var(--git-accent); box-shadow: 0 0 0 1px var(--git-accent); }
```

- 포커스 = 테두리 accent + 바깥 1px accent 링(합쳐서 2px 로 보임). 다른 포커스 표시는 쓰지 않는다.
- 검색칸(`.side-filter`, `.staging-filter`, `.tb-search-wrap`, `.explorer-search`)은 높이 22~28, 돋보기 아이콘 muted, 안쪽 input 은 테두리 없음, `:focus-within` 에서 테두리만 accent. 검색 결과 없음은 테두리 `--git-warning`.
- 아이콘 달린 입력(`.field-with-icon`)은 바깥 상자가 테두리·포커스 링을 갖고 안쪽 `.field` 는 투명.
- placeholder 는 `--git-text-muted`.
- 체크박스(`.tri-check`) 16px, 둥글기 3, 테두리 field-border; hover 테두리 accent; 켜짐은 accent 바탕 + 흰 체크. 글자 달린 체크 `.check` 는 12px, 사이 6px.
- 작은 셀렉트 `.field.small-select` 는 높이 22, 좌우 6.

### 5.4 메뉴 · 팝업

**컨텍스트 메뉴 `.menu`**

```
position fixed · z 30 · min-width 180 · padding 6
둥글기 10 · 바탕 --git-menu-bg · 테두리 1px --git-menu-border
그림자 0 10px 28px menu-shadow, 0 1px 3px menu-shadow
```

**메뉴 항목 `.menu-item`**

| 속성 | 값 |
|---|---|
| 높이 | min 28px, padding `4px 12px 4px 6px`(체크 없는 메뉴는 왼쪽 12) |
| 둥글기 | 6px |
| 글자 | 13px `--git-text`, 왼쪽 정렬, 한 줄 |
| 구성 | [체크 16px] [라벨 flex:1] [힌트 11.5px muted, 왼쪽 18] [▸ 화살 muted, 왼쪽 16] |
| hover / `.active` | 바탕 `--git-menu-hover`, 글자 `--git-menu-hover-text` |
| `.danger` | 글자 `--git-danger` |
| `:disabled` | 글자 muted, `opacity 0.75` |
| 구분선 `.menu-sep` | 1px `--git-menu-border`, 위아래 5px, 좌우 8px |

**고르기 팝업 `.tb-popup`(브랜치·레포지토리)** — 둥글기 8, 같은 바탕·테두리, 그림자 `0 8px 24px`. 맨 위 검색칸, 줄 `.tb-item` 28px 둥글기 5, 선택 줄은 `--git-menu-hover`, 현재 항목 700, 힌트 11px muted.

**작업 목록 `.jobs-pop`** — 380px, 머리/바닥은 `--git-dialog-divider` 로 나눈다. 줄 아이콘은 28px 원, 완료 success-soft/success, 실패 danger-soft/danger, 진행 막대 4px 알약 `--jobs-track` 위 `--git-accent`.

### 5.5 탭 막대 · 도구 막대 · 푸터

- **탭 막대**: `--git-titlebar-bg`(가장 어두운 층), 높이 38, 탭은 아래 정렬. 탭 32px, 둥글기 `8px 8px 0 0`, 13px muted, 사이 1px 세로선. hover `--git-titlebar-hover` + text. 활성 탭은 `--git-toolbar-bg` + strong 600, 안쪽 1px 선 3면, 아래 2px 덮어 도구 막대와 이어진다. 로딩 중 활성 탭 이름 위로 `--tab-shine` 빛줄기.
- **도구 막대**: 높이 48, `--git-toolbar-bg`, 아래 1px `--git-border`. 왼쪽 [레포지토리][브랜치] 고르기(캡션 9px muted + 이름 13px 700 + ▾, hover `--git-hover`), 가운데 버튼 묶음(묶음 사이 1px `--git-divider`), 오른쪽 상태 글 11px muted(작업 중엔 text). 아래 가장자리 2px 진행 막대(`--git-accent`).
- **푸터**: 24px, `--git-header-bg`, 위 1px `--git-border`, 글자 11px muted. 버튼은 20px 둥글기 6, hover `--git-hover`. 버전은 1px `--git-border` 상자 10px.

### 5.6 사이드바

| 요소 | 규칙 |
|---|---|
| 바탕 | `--git-sidebar-bg`(다크 `#26282c`, 라이트 `#f7f8fa`) |
| 위 탭 `.side-tab` | 28px, 좌우 12, text; hover `--git-hover`; 켜짐 `--git-selected` + selected-text; 오른쪽 개수 뱃지 `--git-badge-bg` 둥글기 8, 10px/16px. 묶음 아래 1px `--git-border` |
| 필터 | 위 8px, 높이 26, field 모양 |
| 구역 머리 `.side-section` | 28px, 12px 700 `--git-text-secondary`, hover 글자 text, 왼쪽 ▸ 12px muted(열림 90°), 오른쪽 개수 11.5px 600 `--git-section-count`, hover 시 개수 대신 초록 `+` |
| 항목 `.side-item` | 28px, 13px text, 아이콘 muted, hover `--git-hover`, 포커스 `--git-selected`, 포커스 잃음 `--git-current-row`, 폴더 이름 secondary |
| 현재 브랜치 `.current` | 바탕 `--git-success-soft` + 왼쪽 3px `--git-success` 막대, 이름 700 strong, hover 는 success 34% |
| 구분선 `.side-divider` | 1px `--git-divider`, 좌우 10px |
| 레일(접힘) | 36px, 아이콘 4px 둥글기, 선택 `--git-selected` |

### 5.7 파일 목록 · 줄

- 줄 20px, 왼쪽 4 오른쪽 8, hover `--git-hover`, 선택 `--git-selected` + `--git-selected-text`. 활성(미선택) 줄은 hover 색 + 왼쪽 2px accent 안쪽 선.
- 구성: [▸ 16px] [상태 아이콘 12px] [종류 아이콘] [이름 text] [경로 11.5px muted, rtl 말줄임] [접힌 폴더 요약: 아이콘 + 수 12px 500 strong].
- **줄 hover 동작 버튼은 두지 않는다.** Stage/Unstage 는 머리 줄의 무채색 버튼과 더블클릭·단축키로만.
- 패널 머리 `.stage-header` 32px `--git-header-bg`, 제목 12px 700, 왼쪽 끝 돋보기(켜지면 accent).
- 탐색기 커밋 칸·변경 칸의 파일 줄은 둥글기 5(`.chx-files .file-row`, `.pending-list .file-row`), 커밋 칸 파일(`.chx-file`)은 24px 링크 모양: hover 시 경로가 `--git-accent-fg` 로 바뀌고 뒤에 → 가 나온다.

### 5.8 탐색기

- 머리 줄 24px, 10px muted, 아래 1px `--git-border`. 칸 경계 손잡이 8px, hover/끌기 시 2px `--git-accent`. 칸 순서 끌기 유령은 header-bg + 1px border + `--git-graph-shadow`.
- 커밋 줄 34px. 제목 13px `--git-graph-subject`, [작성자·해시·날짜] 묶음 12px muted. hover `--git-hover`, 선택 `rgba(59,130,196,0.3)` + text. 범위 밖 커밋은 opacity 0.3.
- 탐색기 막대 `.explorer-bar` 32px `--git-header-bg`: 기간 체크 · 요약 | 찾기 220px · [다음] · 탐색기 모양.
- 아래 칸 `.chx-detail`: `--git-sidebar-bg`, 위 탭 줄 32px 에 `.chx-tab`(24px, 둥글기 6, 켜짐 selected). 안내 줄 12px muted, 오류는 danger + danger-soft.
- 커밋 칸 `.chx-info`: 작성자 아바타 36px 둥글기 6(작은 것 20px/4px, 커미터 16px), 이름 14px 700, 이메일·날짜 12px muted, 라벨 72px 오른쪽 맞춤 11px muted, SHA·부모는 `--mono` 12px(hover accent-fg), 제목 15px 700, 본문 `--mono` 12px. 구역 사이 위 1px `--git-border` + 10px.
- 변경·파일 트리 칸 머리 줄 32px 12px: [아바타][작성자 text][해시 mono muted][날짜 muted][제목 text]. 왼쪽 파일 칸 320(180~640), 오른쪽 뷰어 `--git-diff-bg`. 내용 보기 줄 번호 칸 52px `--git-diff-gutter`.

### 5.9 모달 · 시트 · 경고 카드 · 드로어

| | `.modal` | `.modal.sheet` | `.modal.card.confirm.alert`(macOS 경고 모양) | `.drawer` |
|---|---|---|---|---|
| 폭 | 440 (wide 560, settings 640/780) | 같음 | 260 | 사이드바 옆 |
| 바탕 | `--git-menu-bg` | `--git-dialog-bg` | `--git-menu-bg` | `--git-bg` |
| 테두리 | 1px `--git-menu-border` | 1px `--git-dialog-border` | 1px `--git-menu-border` | 오른쪽 1px `--git-border` |
| 둥글기 | 10 | 12 | 14 | 0 |
| 그림자 | `0 12px 40px menu-shadow` | 같음 | 같음 | `8px 0 24px menu-shadow` |
| 안쪽 | 18px, 항목 사이 12 | 머리 `16 20 12`, 본문 `4 20 18`, 바닥 `12 20` + 위 1px dialog-divider | `18 16 16`, 세로 쌓기 | 18px |
| 제목 | 15px 700 strong | 같음 | 13px, 위에 56px 원형 아이콘 | 16px text + 20px 아이콘 |
| 본문 | secondary, pre-wrap, 선택 가능 | | secondary | |
| 라벨 | 12px 600 | | | 12px text |
| 오류 | 12px danger, min-height 16 | | | min-height 16 |
| 버튼 줄 | 오른쪽 정렬, 사이 8 | 바닥 버튼 높이 30 min-width 80 | 둘을 같은 폭으로, 높이 26, 둥글기 7, 사이 8, × 없음 | |
| 뒤 막 | `rgba(0,0,0,0.45)` | 같음 | 같음 | 없음(클립) |

브랜치 카드 양식은 라벨 130px 오른쪽 맞춤 + 입력 28px, 오류·체크는 라벨 폭만큼 들여 쓴다. 드로어는 `cubic-bezier(0.075, 0.82, 0.165, 1)` 0.4s 로 옆에서 밀려 나온다.

### 5.10 토스트 · 진행 카드

창 아래 가운데에 쌓인다. 둥글기 10, 바탕·테두리·그림자는 메뉴와 같되 그림자 `0 10px 30px`. 안쪽 `12 36 12 14`, 13px/1.45 text, 아이콘은 종류별(기본 accent-fg · 성공 success · 경고 warning · 오류 danger). 닫기 ✕는 hover 시만 보임. 들어올 때 240ms `cubic-bezier(0.2,0.8,0.2,1)`, 나갈 때 200ms 아래로 가라앉음.

### 5.11 뱃지 · 이름표 · 칩

| 요소 | 모양 |
|---|---|
| 도구 막대 수 뱃지 `.tb-badge` | 13px 알약, accent 바탕 흰 9px 700(`.danger` 는 danger) |
| 작업 탭 뱃지 `.tab-jobs-badge` | 15px 알약, accent, 10px 700; 완료 점 8px success + 2px titlebar 링 |
| 사이드바 탭 뱃지 `.side-tab-badge` | `--git-badge-bg`, text, 10px/16px, 둥글기 8 |
| 무채색 뱃지 `.recent-badge` | `--git-pressed` 바탕, secondary 글자, 10.5px 500, 둥글기 4 |
| 참조 이름표 `.chx-ref` | 둥글기 4, 11px 흰 글자, 브랜치 pill-bg · 현재 pill-head · 태그 pill-tag |
| 병합 A/B 뱃지 `.mbadge` | 20px 정사각, 둥글기 3, 1px 테두리와 글자가 같은 색(`#2fa4c2` / `#d9a732`) |
| 푸터 버전 | 16px, 10px muted, 1px `--git-border` 둥글기 6 |
| 경고 띠 `.warning`, `.op-banner` | warning-soft 바탕, warning 글자, 둥글기 4 |
| 작성자 경고 `.author-notice` | 왼쪽 3px warning 막대 + header-bg |

### 5.12 Diff · 코드 편집 영역

- 본문 `--mono` 12px/18px, `tab-size 4`, 바탕 `--git-diff-bg`, 글 `--git-diff-text`, 줄 번호 `--git-diff-number`.
- 추가/삭제 줄은 line 색, 그 안의 바뀐 글자는 change 색으로 한 단계 더 진하게. 파일 보기 모드는 줄 번호 칸을 물들이지 않는다.
- **헝크 머리**: 바탕 없이 muted 글자, 아래 1px `--git-border`, Hunk 보기면 30px 에 [Stage Hunk][Discard Hunk] 의미색 3단 버튼.
- **덩어리 hover 띠**: 마우스를 올린 덩어리(위아래 문맥 2줄 포함)를 accent 1px 테두리 + `rgba(80,140,230,0.10)` 로 감싸고, 그 옆에 붙은 묶음 [Stage | 되돌리기…]은 `--git-pressed` 바탕에 field-border, 왼쪽/오른쪽만 둥글게. hover 시 테두리 muted, active `--git-hover`.
- 거터 버튼 `.gut-btn` 16px, success/danger 꽉 찬 바탕 흰 글자, hover 시만.
- 편집기: 현재 줄 `--git-editor-current-line`, 들여쓰기 안내선 `--git-editor-indent-guide`, 캐럿 `--git-editor-caret`, 현재 줄 번호 `--git-editor-number-current`.

### 5.13 로딩 · 전환

- 스켈레톤: 목록 바탕으로 덮고 `--git-skeleton` 막대(점 20px 원, 막대 10px 둥글기 3), 위에서 아래로 2.3s 물결.
- 전환 HUD: 44px 12살 스피너(`--hud-blade`), 이름 19px 600, 부제 13px `--hud-sub`. 들어올 때 scale 0.94→1 320ms 스프링(`cubic-bezier(0.34,1.4,0.5,1)`).
- 화면 전환 `#main.switch-in` 260ms, 살짝 떠오르듯(3px).
- 모든 애니메이션은 `prefers-reduced-motion: reduce` 에서 끄거나 느리게 한다.

---

## 6. 상태 규칙 요약

| 상태 | 무채색 컨트롤(머리 줄 버튼 포함) | 의미색 컨트롤 | 줄·항목 |
|---|---|---|---|
| 기본 | 투명 또는 button-bg, 1px field/button-border, 글자 text/muted | soft 바탕 + base 테두리 | 투명 |
| hover | `--git-hover`(기본 버튼은 `--git-button-hover`), 글자 text | strong 바탕 + 흰 글자 | `--git-hover` |
| active/pressed | `--git-pressed` | strong-pressed | — |
| 선택/켜짐 | `--git-selected` + selected-text | — | `--git-selected` + selected-text |
| 포커스 잃은 선택 | — | — | `--git-current-row` |
| focus(입력) | 테두리 accent + 1px 링 | — | — |
| 현재(브랜치) | — | — | success-soft + 왼쪽 3px success |
| disabled | opacity 0.35~0.5 | opacity 0.5 | — |
| 작업 중 | 링 스피너, 캡션 accent | — | — |

---

## 7. 새 에디터를 만들 때 체크리스트

1. **토큰만 쓴다.** 색은 `--git-*` 변수로만 적고, 고정 hex 는 테마와 무관한 것(툴팁 검정, 병합 A/B, 파일 종류 아이콘, 가지 8색)에만 쓴다.
2. **테마 두 벌을 같이 정의한다.** `:root[data-theme="dark"]`·`:root[data-theme="light"]` 둘 다. 다크 패널 안이면 `.sidebar`/`.detail` 의 한 단계 밝은 재정의도 챙긴다.
3. **바탕 층은 밝기 차로.** 그림자는 메뉴·모달·토스트·드로어·HUD 에만.
4. **치수는 사다리에서 고른다.** 높이 20/22/24/26/28/32/48, 여백 4/6/8/10/12/18/20, 둥글기 3/4/5/6/8/10/12/14/24, 글자 9~15px.
5. **자주 쓰는 조작은 무채색.** Stage/Unstage 급 버튼은 외곽선만. 의미색은 되돌릴 수 없는 것·상태 표시에만.
6. **의미색은 4값 세트로.** base·soft·strong·strong-pressed 를 함께 정의하고 "soft → strong(hover) → pressed" 규칙을 지킨다.
7. **포커스는 accent 링 하나.** 다른 outline 을 만들지 않는다.
8. **글자 위계 4단.** strong / text / secondary / muted 밖의 글자색을 만들지 않는다.
9. **글꼴 굵기 400·500·600·700 만.** 글꼴은 `--font`, 코드는 `--mono`.
10. **텍스트 선택은 꺼 두고** 입력·코드·메시지에서만 켠다.
11. **reduced-motion 을 존중한다.**

### 7.1 최소 토큰 묶음(새 에디터 시작용)

새 에디터가 가장 먼저 받아야 할 토큰 18개:

```
바탕   --git-bg  --git-sidebar-bg  --git-header-bg  --git-menu-bg  --git-field-bg
글자   --git-text-strong  --git-text  --git-text-secondary  --git-text-muted
선     --git-border  --git-field-border  --git-menu-border
상태   --git-hover  --git-pressed  --git-selected  --git-selected-text
강조   --git-accent  --git-accent-fg
```

여기에 의미색 세트(success·warning·danger × 4), diff 8색, 문법색을 필요한 만큼 더한다.
