---
name: unity-editor-uitk
description: 유니티 에디터 도구(EditorWindow, Inspector, Overlay, 설정 화면)의 UI 를 만들거나 고칠 때 쓴다. UI Toolkit 으로만 만들고 UXML·USS·C# 를 분리하며, 기능이나 UI 컴포넌트 단위로 파일을 쪼개고 재사용되는 부분은 별도 컴포넌트로 뽑는다. IMGUI 나 uGUI 로 에디터 UI 를 만들려 할 때, 한 파일에 UI 를 몰아 넣으려 할 때도 이 스킬을 따른다.
---

# 유니티 에디터 UI 제작 규칙 (UI Toolkit)

에디터 UI 는 **UI Toolkit 으로만** 만들고, **UXML(구조) · USS(스타일) · C#(동작) 을 반드시 분리**한다. 한 화면을 한 파일에 몰아 넣지 않는다.

## 1. 반드시 지키는 규칙

| # | 규칙 |
|---|---|
| 1 | 에디터 UI 는 UI Toolkit 만 쓴다. `OnGUI`, `EditorGUILayout`, `GUILayout`, IMGUIContainer, uGUI 로 에디터 화면을 만들지 않는다. 기존 IMGUI 를 고칠 때도 새로 붙이는 부분은 UI Toolkit 으로 쓴다. |
| 2 | 구조는 UXML, 스타일은 USS, 동작은 C#. 세 가지를 한 파일에 섞지 않는다. |
| 3 | C# 에서 `new VisualElement()` 로 구조를 길게 조립하지 않는다. 구조는 UXML 에 둔다. 동적으로 개수가 변하는 목록 항목처럼 UXML 로 못 쓰는 부분만 코드로 만든다. |
| 4 | C# 에서 `element.style.xxx = ...` 로 모양을 정하지 않는다. 모양은 USS 클래스로 정하고, 코드는 `AddToClassList` / `RemoveFromClassList` / `EnableInClassList` 로 상태만 바꾼다. 위치·크기를 계산해야 하는 경우만 예외다. |
| 5 | UXML 에 `style="..."` 인라인 스타일을 쓰지 않는다. |
| 6 | 색·간격·글자 크기·둥글기는 USS 변수(`--변수명`)로만 쓴다. 숫자와 hex 를 USS 규칙 안에 직접 적지 않는다. 변수는 테마 USS 한 곳에서 정의한다. |
| 7 | 화면 하나, 컴포넌트 하나를 파일 한 벌(UXML + USS + C#)로 만든다. 한 UXML 이나 한 C# 클래스가 여러 기능을 맡지 않는다. |

## 2. 파일을 쪼개는 기준

아래 중 **하나라도 해당하면 분리한다.** 애매하면 분리한다.

- **기능이 다르다**: 목록, 검색, 상세, 설정처럼 사용자가 다른 일을 하는 영역.
- **UI 컴포넌트로 읽힌다**: 툴바, 메뉴 항목, 탭 막대, 카드, 입력 줄, 뱃지, 다이얼로그.
- **두 번 이상 쓰인다**: 같은 모양이 다른 화면에도 나오거나 나올 것이 보인다.
- **상태와 이벤트가 독립적이다**: 부모가 몰라도 되는 상태를 스스로 갖는다.
- **파일이 커진다**: UXML 이 약 100줄, C# 클래스가 약 200줄을 넘어가면 쪼갤 곳을 찾는다.

쪼개지 않아도 되는 경우: 한 화면에서만 쓰고, 상태가 부모와 완전히 붙어 있고, 몇 줄짜리 정적 구조일 때.

### 분리 방식

| 상황 | 방법 |
|---|---|
| 재사용되는 정적 조각 | 별도 UXML 을 만들고 부모 UXML 에서 `<ui:Instance template="..."/>` 로 끼운다. |
| 자기 상태·이벤트가 있는 조각 | `VisualElement` 를 상속한 커스텀 컴포넌트 클래스 + 자기 UXML + 자기 USS. |
| 화면 단위 | `EditorWindow` 는 컴포넌트를 조립만 한다. 화면 로직은 얇게 유지한다. |
| 여러 컴포넌트가 공유하는 모양 | `Common` USS 로 올린다. 컴포넌트 USS 에 복사해 넣지 않는다. |

## 3. 폴더 구조

```
Editor/
  UI/
    Theme/
      Theme.Dark.uss          # :root 변수 정의 (다크)
      Theme.Light.uss         # :root 변수 정의 (라이트)
      Common.uss              # 여러 컴포넌트가 공유하는 규칙
    Components/               # 재사용 컴포넌트, 컴포넌트마다 폴더 하나
      SearchField/
        SearchField.cs
        SearchField.uxml
        SearchField.uss
      TabBar/
        TabBar.cs
        TabBar.uxml
        TabBar.uss
    Windows/                  # 화면 단위
      MyToolWindow/
        MyToolWindow.cs       # EditorWindow, 조립만
        MyToolWindow.uxml
        MyToolWindow.uss
        Panels/               # 이 화면에서만 쓰는 영역
          ListPanel/ (cs, uxml, uss)
          DetailPanel/ (cs, uxml, uss)
```

- 컴포넌트 하나는 **같은 이름의 `.cs` / `.uxml` / `.uss` 세 파일**이 한 폴더에 있다.
- 한 화면에서만 쓰는 영역은 그 화면의 `Panels/` 에 둔다. 두 번째 쓰임이 생기면 `Components/` 로 옮긴다.
- 에디터 코드이므로 `Editor` 폴더 안, 또는 Editor 전용 asmdef 에 둔다.

## 4. 컴포넌트 작성 패턴

### UXML (구조만)

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements" xmlns:uie="UnityEditor.UIElements">
    <ui:VisualElement name="search-field" class="search-field">
        <ui:Label name="icon" class="search-field__icon" text="🔍" />
        <ui:TextField name="input" class="search-field__input" />
        <ui:Button name="clear" class="search-field__clear" text="✕" />
    </ui:VisualElement>
</ui:UXML>
```

### USS (모양만, 변수만 사용)

```css
.search-field {
    flex-direction: row;
    align-items: center;
    height: var(--ctl-h);
    padding: 0 var(--space-2);
    background-color: var(--color-field-bg);
    border-width: 1px;
    border-color: var(--color-field-border);
    border-radius: var(--radius);
}
.search-field:focus-within { border-color: var(--color-accent); }
.search-field__clear { display: none; }
.search-field--has-text .search-field__clear { display: flex; }
```

### C# (동작만)

Unity 6 이상은 `[UxmlElement]`, 이전 버전은 `UxmlFactory` 를 쓴다.

```csharp
using System;
using UnityEditor;
using UnityEngine.UIElements;

namespace MyTool.UI
{
    [UxmlElement]
    public partial class SearchField : VisualElement
    {
        const string UxmlPath = "Assets/.../SearchField.uxml";
        const string UssPath  = "Assets/.../SearchField.uss";

        public event Action<string> ValueChanged;

        readonly TextField _input;
        readonly Button _clear;

        public SearchField()
        {
            AssetDatabase.LoadAssetAtPath<VisualTreeAsset>(UxmlPath).CloneTree(this);
            styleSheets.Add(AssetDatabase.LoadAssetAtPath<StyleSheet>(UssPath));

            _input = this.Q<TextField>("input");
            _clear = this.Q<Button>("clear");

            _input.RegisterValueChangedCallback(e =>
            {
                EnableInClassList("search-field--has-text", !string.IsNullOrEmpty(e.newValue));
                ValueChanged?.Invoke(e.newValue);
            });
            _clear.clicked += () => _input.value = string.Empty;
        }
    }
}
```

- 이름(`name`)은 UXML 과 C# 이 공유하는 계약이다. 문자열이 흩어지지 않게 클래스 상단 `const` 로 모은다.
- `Q<T>("name")` 결과는 생성자에서 한 번 찾아 필드에 보관한다. 이벤트마다 다시 찾지 않는다.
- 부모에게는 이벤트(`event Action<T>`)로 알린다. 컴포넌트가 부모 구조를 직접 건드리지 않는다.
- 외부에서 쓰는 값은 속성으로 노출하고, 안쪽 요소는 `private` 으로 둔다.
- 에셋 경로는 상수 한 곳에 둔다. 경로가 여러 군데 흩어지지 않게 한다.

## 5. USS 규칙

- **이름은 BEM**: `블록`, `블록__요소`, `블록--상태`. 예: `tab-bar`, `tab-bar__item`, `tab-bar__item--active`.
- 클래스 선택자만 쓴다. 이름(`#name`)이나 타입 선택자로 스타일을 걸지 않는다. `name` 은 C# 이 찾기 위한 것이다.
- 선택자 중첩은 얕게. 자손 선택자는 두 단계를 넘기지 않는다.
- 컴포넌트 USS 는 **자기 클래스만** 건드린다. 자식 컴포넌트 내부 클래스를 덮어쓰지 않는다. 바꿔야 하면 그 컴포넌트에 변형 클래스(`--compact` 등)를 만든다.
- `!important` 는 쓰지 않는다.

### 테마 변수

변수는 `Theme.Dark.uss` / `Theme.Light.uss` 에서만 정의하고, 컴포넌트는 값을 모른 채 변수만 쓴다.

```css
/* Theme.Dark.uss */
:root {
    --color-bg: #191a1c;
    --color-text: #d1d3d9;
    --color-accent: #3871e1;
    --radius: 6px;
    --space-2: 8px;
    --ctl-h: 22px;
}
```

테마는 `EditorGUIUtility.isProSkin` 으로 고른 한 장만 루트에 붙인다. 변수 목록과 값은 [design/DesignSheet.md](../../design/DesignSheet.md) 의 토큰을 따른다.

```csharp
root.styleSheets.Add(EditorGUIUtility.isProSkin ? darkTheme : lightTheme);
root.styleSheets.Add(commonUss);
```

## 6. 작업 순서

1. **화면을 영역으로 나눈다.** 기능별, 컴포넌트별로 나누고 재사용될 것을 표시한다.
2. **재사용 컴포넌트부터 만든다.** `Components/<이름>/` 에 uxml, uss, cs 한 벌.
3. **화면을 조립한다.** 화면 UXML 에서 컴포넌트를 `Instance` 나 커스텀 요소로 배치한다.
4. **스타일을 변수로 정리한다.** 하드코딩 값이 있으면 테마 변수로 올린다.
5. **다크·라이트 둘 다 확인한다.**

## 7. 하지 말 것

| 안 좋은 모양 | 이렇게 한다 |
|---|---|
| `EditorWindow` 안에서 `new Label()`, `new Button()` 수십 개를 조립 | UXML 로 옮기고 C# 은 `Q<>` 와 이벤트 연결만 |
| 한 UXML 에 툴바·목록·상세·설정을 전부 | 영역마다 UXML + USS + C# 로 분리 |
| `label.style.color = Color.red` | USS 클래스 `--error` 를 만들고 `AddToClassList` |
| 같은 버튼 모양을 두 화면 USS 에 복사 | `Components/` 로 뽑거나 `Common.uss` 로 올리기 |
| USS 에 `#3871e1`, `8px` 직접 기입 | `var(--color-accent)`, `var(--space-2)` |
| UXML 에 `style="margin: 4px"` | USS 클래스로 |
| 컴포넌트가 `parent.parent.Q(...)` 로 바깥 구조 접근 | 이벤트로 부모에게 알리기 |
| 클래스 이름 `.title`, `.row` 처럼 범용 이름 | `.card__title`, `.list__row` 처럼 블록 이름을 붙이기 |

## 8. 마무리 점검

- [ ] UI 코드에 IMGUI(`OnGUI`, `EditorGUILayout`)가 없다
- [ ] 모든 화면·컴포넌트가 `.uxml` + `.uss` + `.cs` 로 나뉘어 있다
- [ ] UXML 에 인라인 `style` 이 없고, C# 에 `style.xxx` 대입이 없다(계산이 필요한 위치·크기 제외)
- [ ] USS 규칙 안에 hex 색과 px 숫자가 직접 쓰이지 않고 변수를 쓴다
- [ ] 한 파일이 여러 기능을 맡지 않는다. 100줄 넘는 UXML, 200줄 넘는 클래스는 쪼갤 곳을 확인했다
- [ ] 두 곳 이상에서 쓰는 모양은 컴포넌트나 `Common.uss` 로 올렸다
- [ ] `name` 문자열이 C# 상수 한 곳에 모여 있다
- [ ] 다크·라이트 테마 둘 다에서 확인했다
