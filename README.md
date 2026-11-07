# Unity Editor Philosophy

유니티 에디터 도구를 만드는 방식에 대한 기준을 모아 두는 레포지토리입니다.

## 구성

| 폴더 | 내용 | 상태 |
|---|---|---|
| [`design/`](design/) | 에디터 UI 디자인: 색상 토큰, 컴포넌트 스타일, UI 컨셉 | 작성 중 |
| [`skills/`](skills/) | 에디터 코드를 작성할 때 쓰는 코드 스킬 | 작성 중 |

## 디자인

- [DesignSheet.md](design/DesignSheet.md): 다크·라이트 에디터 디자인 시트. 색상 팔레트, 메뉴·바디·버튼 등 컴포넌트 규칙, UI 컨셉을 정의합니다.

## 코드 스킬

- [unity-editor-uitk](skills/unity-editor-uitk/SKILL.md): UI Toolkit 으로만 제작하고 UXML · USS · C# 를 분리한다. 기능과 UI 컴포넌트 단위로 파일을 쪼개고, 재사용되는 부분은 컴포넌트로 뽑는다.
