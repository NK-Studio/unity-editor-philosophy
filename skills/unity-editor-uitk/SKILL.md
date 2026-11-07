---
name: unity-editor-uitk
description: 유니티 에디터 도구(EditorWindow, 커스텀 Inspector, PropertyDrawer, 설정 화면)의 UI 를 만들거나 고칠 때 쓴다. UI Toolkit 으로만 만들고 UXML·USS·C# 를 분리하며, 기능이나 UI 컴포넌트 단위로 파일을 쪼개고 재사용되는 부분은 별도 컴포넌트로 뽑는다. IMGUI(OnGUI·EditorGUILayout)로 에디터 UI 를 만들거나 고치려 할 때, 한 파일에 UI 를 몰아 넣으려 할 때도 이 스킬을 따른다.
---

# 유니티 에디터 UI 제작 규칙 (UI Toolkit)

에디터 UI 는 **UI Toolkit 으로만** 만들고, **UXML(구조) · USS(스타일) · C#(동작) 을 반드시 분리**한다. 한 화면을 한 파일에 몰아 넣지 않는다.

적용 범위: `EditorWindow`, `Editor`(커스텀 Inspector), `PropertyDrawer`, `SettingsProvider`, `Overlay` 등 에디터 안에서 그리는 모든 UI.

## 1. 반드시 지키는 규칙

| # | 규칙 |
|---|---|
| 1 | 에디터 UI 는 UI Toolkit 만 쓴다. `OnGUI`, `OnInspectorGUI`, `EditorGUILayout`, `GUILayout`, `IMGUIContainer` 로 새 화면을 만들지 않는다. 기존 IMGUI 코드를 고칠 때도 새로 붙이는 부분은 UI Toolkit 으로 쓰고, 가능하면 그 화면을 통째로 옮긴다. |
| 2 | 구조는 UXML, 스타일은 USS, 동작은 C#. 세 가지를 한 파일에 섞지 않는다. |
| 3 | C# 에서 `new VisualElement()` 로 구조를 길게 조립하지 않는다. 구조는 UXML 에 둔다. 개수가 바뀌는 목록 항목처럼 UXML 로 못 쓰는 부분만 코드로 만들고, 그때도 항목 하나는 자기 UXML 을 갖는다. |
| 4 | C# 에서 `element.style.xxx = ...` 로 모양을 정하지 않는다. 모양은 USS 클래스로 정하고, 코드는 `AddToClassList` / `RemoveFromClassList` / `EnableInClassList` 로 상태만 바꾼다. 마우스 위치나 드래그처럼 계산한 좌표·크기를 넣어야 하는 경우만 예외다. |
| 5 | UXML 에 `style="..."` 인라인 스타일을 쓰지 않는다. |
| 6 | 색·간격·글자 크기·둥글기는 USS 변수(`--변수명`)로만 쓴다. 컴포넌트 USS 규칙 안에 hex 와 px 숫자를 직접 적지 않는다. 변수는 테마 USS 한 곳에서 정의한다. |
| 7 | 화면 하나, 컴포넌트 하나를 파일 한 벌(UXML + USS + C#)로 만든다. 한 UXML 이나 한 C# 클래스가 여러 기능을 맡지 않는다. |
| 8 | 데이터는 바인딩으로 잇는다. `SerializedObject` 를 다루는 Inspector·PropertyDrawer 는 UXML 의 `binding-path` 와 `Bind()` 를 쓰고, 값을 손으로 읽어 Label 에 써 넣지 않는다. |

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
| 재사용되는 정적 조각 | 별도 UXML 을 만들고 부모 UXML 에서 `<ui:Template>` 로 선언한 뒤 `<ui:Instance>` 로 끼운다. |
| 자기 상태·이벤트가 있는 조각 | `VisualElement` 를 상속한 커스텀 컴포넌트 클래스 + 자기 UXML + 자기 USS. UXML 에서 태그로 바로 쓸 수 있게 `[UxmlElement]` 를 붙인다. |
| 개수가 바뀌는 목록 | `ListView` / `MultiColumnListView` 의 `makeItem` / `bindItem` 을 쓴다. 항목 하나는 자기 UXML 을 가진 컴포넌트다. |
| 화면 단위 | `EditorWindow` 와 `Editor` 는 컴포넌트를 조립만 한다. 화면 로직은 얇게 유지한다. |
| 여러 컴포넌트가 공유하는 모양 | `Common.uss` 로 올린다. 컴포넌트 USS 에 복사해 넣지 않는다. |

## 3. 폴더 구조

```
Editor/                       # Editor 전용 asmdef (Include Platforms: Editor)
  UI/
    UiAssets.cs               # UXML·USS 로딩 헬퍼, 에셋 루트 경로 상수 한 곳
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
    Inspectors/               # 커스텀 Inspector · PropertyDrawer
      MyAssetInspector/ (cs, uxml, uss)
```

- 컴포넌트 하나는 **같은 이름의 `.cs` / `.uxml` / `.uss` 세 파일**이 한 폴더에 있다.
- 한 화면에서만 쓰는 영역은 그 화면의 `Panels/` 에 둔다. 두 번째 쓰임이 생기면 `Components/` 로 옮긴다.
- 에디터 코드이므로 `Editor` 폴더 안이면서 Editor 전용 asmdef 에 둔다. 런타임 어셈블리에서 참조하지 않는다.

## 4. 에셋 로딩

UXML·USS 경로가 코드 여기저기에 흩어지지 않게 **헬퍼 한 곳**에서만 읽는다. 패키지면 `Packages/<패키지 이름>/...`, 프로젝트면 `Assets/...` 로 루트만 바꾼다. 못 찾으면 조용히 `null` 로 넘어가지 않고 바로 예외를 던진다.

```csharp
using System;
using UnityEditor;
using UnityEngine.UIElements;

namespace MyTool.UI
{
    static class UiAssets
    {
        const string Root = "Packages/com.mystudio.mytool/Editor/UI/";

        public static VisualTreeAsset Uxml(string relativePath) => Load<VisualTreeAsset>(relativePath + ".uxml");
        public static StyleSheet Uss(string relativePath) => Load<StyleSheet>(relativePath + ".uss");

        static T Load<T>(string relativePath) where T : UnityEngine.Object
        {
            var asset = AssetDatabase.LoadAssetAtPath<T>(Root + relativePath);
            if (asset == null) throw new InvalidOperationException($"UI asset not found: {Root}{relativePath}");
            return asset;
        }

        public static void ApplyTheme(VisualElement root)
        {
            root.styleSheets.Add(Uss(EditorGUIUtility.isProSkin ? "Theme/Theme.Dark" : "Theme/Theme.Light"));
            root.styleSheets.Add(Uss("Theme/Common"));
        }
    }
}
```

`EditorWindow` 는 Unity 가 만들어 주는 `[SerializeField] VisualTreeAsset` 필드로 받아도 된다. 어느 쪽이든 한 프로젝트 안에서는 한 방식만 쓴다.

## 5. 컴포넌트 작성 패턴

### UXML (구조만)

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements" xmlns:uie="UnityEditor.UIElements">
    <ui:VisualElement name="root" class="search-field">
        <ui:VisualElement name="icon" class="search-field__icon" />
        <ui:TextField name="input" class="search-field__input" />
        <ui:Button name="clear" class="search-field__clear" />
    </ui:VisualElement>
</ui:UXML>
```

- 아이콘은 `text="🔍"` 같은 글자가 아니라 USS `background-image` 로 넣는다.
- `name` 은 C# 이 찾기 위한 것이고, `class` 는 USS 가 꾸미기 위한 것이다. 둘을 섞어 쓰지 않는다.

### USS (모양만, 변수만 사용)

```css
.search-field {
    flex-direction: row;
    align-items: center;
    height: var(--ctl-h);
    padding: 0 var(--space-2);
    /* 6.7 이상: 자식 사이 간격은 gap. 그 아래 버전은 자식 margin 으로. */
    column-gap: var(--space-1);
    background-color: var(--color-field-bg);
    border-width: 1px;
    border-color: var(--color-field-border);
    border-radius: var(--radius);
}
.search-field--focused { border-color: var(--color-accent); }
.search-field__icon {
    width: 16px;
    height: 16px;
    background-image: var(--unity-icons-search);
}
.search-field__input { flex-grow: 1; }
.search-field__clear {
    display: none;
    width: 16px;
    height: 16px;
    background-image: var(--unity-icons-clear);
}
.search-field--has-text .search-field__clear { display: flex; }
```

- USS 에는 `:focus-within` 이 **없다.** 지원되는 의사 클래스는 `:hover` `:active` `:inactive` `:focus` `:selected` `:disabled` `:enabled` `:checked` `:root` 뿐이다. 컨테이너의 포커스 상태는 C# 에서 `FocusInEvent` / `FocusOutEvent` 를 받아 클래스를 켜고 끈다.
- 에디터 내장 아이콘은 Unity 기본 테마가 정의한 `--unity-icons-*` 변수로 가져온다. 직접 그린 아이콘은 `resource()` 나 `url()` 로 넣는다.
- 아이콘 크기(16px)처럼 그림 자체의 치수는 변수가 아니어도 된다. 간격·색·둥글기는 변수다.

### C# (동작만)

Unity 6(2023.2 이상)은 `[UxmlElement]` + `partial` 로 UXML 태그를 만든다. 그 이전 버전은 `UxmlFactory<T, UxmlTraits>` 중첩 클래스를 쓴다.

```csharp
using System;
using UnityEngine.UIElements;

namespace MyTool.UI
{
    [UxmlElement]
    public partial class SearchField : VisualElement
    {
        const string AssetPath = "Components/SearchField/SearchField";
        const string ClassFocused = "search-field--focused";
        const string ClassHasText = "search-field--has-text";

        public event Action<string> ValueChanged;

        readonly TextField _input;
        readonly Button _clear;

        public string Value
        {
            get => _input.value;
            set => _input.value = value;
        }

        public SearchField()
        {
            UiAssets.Uxml(AssetPath).CloneTree(this);
            styleSheets.Add(UiAssets.Uss(AssetPath));

            _input = this.Q<TextField>("input");
            _clear = this.Q<Button>("clear");

            _input.RegisterValueChangedCallback(OnInputChanged);
            _input.RegisterCallback<FocusInEvent>(_ => AddToClassList(ClassFocused));
            _input.RegisterCallback<FocusOutEvent>(_ => RemoveFromClassList(ClassFocused));
            _clear.clicked += () => _input.value = string.Empty;
        }

        void OnInputChanged(ChangeEvent<string> e)
        {
            EnableInClassList(ClassHasText, !string.IsNullOrEmpty(e.newValue));
            ValueChanged?.Invoke(e.newValue);
        }
    }
}
```

- `name` 과 클래스 이름은 UXML·USS 와 C# 이 공유하는 계약이다. 문자열이 흩어지지 않게 클래스 상단 `const` 로 모은다.
- `Q<T>("name")` 결과는 생성자에서 한 번 찾아 필드에 보관한다. 이벤트마다 다시 찾지 않는다.
- 부모에게는 이벤트(`event Action<T>`)로 알린다. 컴포넌트가 `parent` 를 타고 올라가 바깥 구조를 건드리지 않는다.
- 외부에서 쓰는 값은 속성으로 노출하고, 안쪽 요소는 `private` 으로 둔다.
- 컴포넌트의 USS 는 컴포넌트 자신에게 붙인다. 테마 USS 는 창 루트에 한 번만 붙인다.

### 화면 (EditorWindow)

```csharp
using UnityEditor;
using UnityEngine.UIElements;

namespace MyTool.UI
{
    public class MyToolWindow : EditorWindow
    {
        const string AssetPath = "Windows/MyToolWindow/MyToolWindow";

        [MenuItem("Window/My Tool")]
        static void Open() => GetWindow<MyToolWindow>("My Tool");

        public void CreateGUI()
        {
            var root = rootVisualElement;
            UiAssets.ApplyTheme(root);
            UiAssets.Uxml(AssetPath).CloneTree(root);
            root.styleSheets.Add(UiAssets.Uss(AssetPath));

            var search = root.Q<SearchField>("search");
            var list = root.Q<ListPanel>("list");
            search.ValueChanged += list.SetFilter;
        }
    }
}
```

`CreateGUI()` 는 조립과 이벤트 연결만 한다. 데이터 처리와 상태는 패널·컴포넌트가 갖는다.

### Inspector · PropertyDrawer

```csharp
[CustomEditor(typeof(MyAsset))]
public class MyAssetInspector : Editor
{
    const string AssetPath = "Inspectors/MyAssetInspector/MyAssetInspector";

    public override VisualElement CreateInspectorGUI()
    {
        var root = new VisualElement();
        UiAssets.Uxml(AssetPath).CloneTree(root);
        root.styleSheets.Add(UiAssets.Uss(AssetPath));
        root.Bind(serializedObject);   // UXML 의 binding-path 가 필드에 붙는다
        return root;
    }
}
```

```xml
<uie:PropertyField binding-path="displayName" />
<ui:Toggle binding-path="enabled" class="my-asset__toggle" />
```

- `OnInspectorGUI()` 를 재정의하지 않는다. `CreateInspectorGUI()` 가 `VisualElement` 를 돌려준다.
- `PropertyDrawer` 는 `CreatePropertyGUI(SerializedProperty)` 를 쓴다.
- 값은 `binding-path` 와 `Bind()` 로 잇고, `Update()` 와 `ApplyModifiedProperties()` 를 손으로 부르는 코드를 늘어놓지 않는다.

### 템플릿 인스턴스 (정적 조각 재사용)

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements">
    <ui:Template name="Header" src="../Panels/Header/Header.uxml" />
    <ui:VisualElement class="my-window">
        <ui:Instance template="Header" name="header" />
    </ui:VisualElement>
</ui:UXML>
```

`<ui:Template>` 로 먼저 선언해야 `<ui:Instance>` 를 쓸 수 있다. `src` 는 이 UXML 기준 상대 경로다.

## 6. USS 규칙

- **이름은 BEM**: `블록`, `블록__요소`, `블록--상태`. 예: `tab-bar`, `tab-bar__item`, `tab-bar__item--active`.
- 클래스 선택자만 쓴다. 이름(`#name`)이나 타입 선택자(`Button`, `Label`)로 스타일을 걸지 않는다.
- 선택자 중첩은 얕게. 자손 선택자는 두 단계를 넘기지 않는다.
- 컴포넌트 USS 는 **자기 클래스만** 건드린다. 자식 컴포넌트 내부 클래스를 덮어쓰지 않는다. 바꿔야 하면 그 컴포넌트에 변형 클래스(`--compact` 등)를 만든다.
- Unity 내장 컨트롤의 내부 클래스(`.unity-text-field__input` 등)를 덮어쓸 때는 반드시 자기 블록 아래로 한정한다(`.search-field .unity-text-field__input`).
- `!important` 는 쓰지 않는다.
- 전환(transition)은 `translate` · `scale` · `rotate` · 색 · `opacity` 에만 건다. 레이아웃 속성(`width`, `left`, `margin`)에 걸면 매 프레임 레이아웃을 다시 계산한다. 전환은 `:hover` 규칙이 아니라 **기본 규칙**에 둔다(그래야 마우스가 떠날 때도 돌아가는 전환이 생긴다).
- 그림자는 `box-shadow` 가 아니라 `filter: drop-shadow(...)` 다(Unity 6.6 이상). 그 아래 버전은 9-slice 그림자 이미지를 깔거나 그림자를 생략한다.
- 떠 있는 패널 뒤 흐림(frosted glass)은 `backdrop-filter`(Unity 6.6 이상)다. 런타임 URP 전용으로 문서화되어 있으니 **에디터 창에서 쓰려면 먼저 그 버전에서 동작을 확인**하고, 안 되면 반투명 배경색만으로 대신한다.

### 테마 변수

변수는 `Theme.Dark.uss` / `Theme.Light.uss` 에서만 정의하고, 컴포넌트는 값을 모른 채 변수만 쓴다. 변수 이름과 값은 [design/DesignSheet.md](../../design/DesignSheet.md) 의 토큰을 따른다.

```css
/* Theme.Dark.uss */
:root {
    --color-bg: #191a1c;
    --color-text: #d1d3d9;
    --color-field-bg: #191a1c;
    --color-field-border: #40434a;
    --color-accent: #3871e1;
    --radius: 6px;
    --space-2: 8px;
    --ctl-h: 22px;
}
```

- 테마는 `EditorGUIUtility.isProSkin` 으로 고른 한 장만 루트에 붙인다(4절 `ApplyTheme`).
- `var(--x, 기본값)` 처럼 두 번째 인자로 기본값을 줄 수 있다. 변수를 지웠는데 참조가 남아 있을 때를 대비해 테마 밖에서 쓰는 변수에는 기본값을 둔다.
- 변수를 다른 함수 안에 넣을 수 없다(`rgba(var(--r), 0, 0, 1)` 불가). 변수끼리 연산도 안 된다. 필요한 조합은 테마에서 변수 하나로 미리 만든다.
- 에디터 기본 색과 맞춰야 하는 곳(배경·구분선)은 Unity 기본 테마 변수 `--unity-colors-*` 를 우리 변수에 대입해 쓴다. 컴포넌트가 `--unity-colors-*` 를 직접 쓰지는 않는다.

### USS 지원 범위 (Unity 6 기준)

USS 는 CSS 의 부분집합이다. 아래 표 밖의 CSS 를 쓰기 전에 그 버전의 [USS 속성 레퍼런스](https://docs.unity.com/en-us/engine/6000.7/manual/uitoolkits/uielements/uie-uss/properties/reference)에서 확인한다. **프로젝트의 최소 Unity 버전을 정하고, 그보다 늦게 생긴 기능은 쓰지 않는다.**

**선택자·의사 클래스**

| 되는 것 | 안 되는 것 |
|---|---|
| 타입 `Button`, 이름 `#name`, 클래스 `.class`, 전체 `*` | 속성 선택자 `[attr]` |
| 자손 ` `, 자식 `>`, 복합 `Button.a#b`, 목록 `,` | 형제 `+` `~` |
| `:hover` `:active` `:inactive` `:focus` `:disabled` `:enabled` `:checked` `:root` | `:focus-within` `:first-child` `:nth-child` `:not()` `:has()` `:selected`(→ `:checked`) |
| | 의사 요소 `::before` `::after` |

컨테이너의 포커스나 "n번째 줄" 같은 상태는 C# 에서 클래스를 켜고 끈다.

**값·함수**

| 되는 것 | 안 되는 것 |
|---|---|
| `var(--x)`, `var(--x, 기본값)` | 함수 안의 `var()`, 변수 연산 |
| `url("경로")`(USS 파일 기준 상대 경로, `project:/` 절대 경로) | `calc()` |
| `resource("이름")`, `resource("경로#서브에셋")`(6.4 이상) | |
| `rgb()` `rgba()` `#hex`, 색 이름 | |
| `linear-gradient()` `radial-gradient()` (`background-image`, 6.7 이상) | |
| 단위 `px` `%` `deg` `grad` `rad` `turn` `s` `ms` | `em` `rem` `vw` `vh` |

**속성 (항상 되는 것)**

| 묶음 | 속성 |
|---|---|
| 상자 | `width` `height` `min-*` `max-*` `margin*` `padding*` `border-*-width` |
| flex | `flex-direction` `flex-wrap` `flex-grow` `flex-shrink` `flex-basis` `flex` `align-items` `align-self` `align-content` `justify-content` |
| 위치 | `position`(`relative` \| `absolute`) `left` `top` `right` `bottom` |
| 배경·테두리 | `background-color` `background-image` `-unity-background-scale-mode` `-unity-background-image-tint-color` `border-color` `border-*-color` `border-radius` `border-*-radius` `-unity-slice-*` |
| 표시 | `display`(`flex` \| `none`) `visibility` `overflow` `opacity` `cursor` |
| 글자 | `color` `font-size` `white-space` `letter-spacing` `word-spacing` `text-overflow` `text-shadow` `-unity-font` `-unity-font-definition` `-unity-font-style` `-unity-text-align` `-unity-text-outline*` `-unity-paragraph-spacing` |
| 변형 | `translate` `rotate` `scale` `transform-origin` (적용 순서 scale → rotate → translate) |
| 전환 | `transition-property` `transition-duration` `transition-timing-function` `transition-delay` `transition` (easing: `ease*`, `linear`, `ease-in-out-sine/cubic/circ/elastic/back/bounce` 등) |

**버전에 따라 생긴 것**

| 기능 | 버전 | 메모 |
|---|---|---|
| `aspect-ratio`, `-unity-text-auto-size` | 6.3 | |
| `filter`: `blur()` `grayscale()` `invert()` `opacity()` `sepia()` `tint()` `hue-rotate()` `contrast()` `brightness()` `saturate()` | 6.3 | 한 선언 안의 함수는 순서대로 파이프라인. 클래스 두 개가 각각 `filter` 를 주면 마지막 것만 적용(합쳐지지 않음). 요소 서브트리를 텍스처로 그려 처리하므로 큰 영역·많은 요소에는 아낀다 |
| `filter: drop-shadow(x y blur color)` | 6.6 | 렌더된 모양(투명 영역 포함)을 따라가는 그림자. 여러 개 겹치기 가능. 6.6~6.7 베타에서 자식까지 흐려지는 버그 보고가 있어 그림자는 **내용이 없는 별도 요소**에 거는 편이 안전 |
| `backdrop-filter` | 6.6 | 요소 뒤를 흐림·틴트. URP 와 screen-space 패널 전용, 전환 불가, 한 요소에 하나만. 에디터 창 지원은 확인 후 사용 |
| `gap` `row-gap` `column-gap` | 6.7 | flex 자식 사이 간격. 그 아래 버전은 자식 `margin` 으로 |
| `z-index` | 6.7 | 같은 스태킹 컨텍스트 안 겹침 순서 |
| `linear-gradient()` `radial-gradient()` | 6.7 | `background-image` 값 |
| `display: grid` 와 `grid-*` | 6.7 실험 | Project Settings > UI Toolkit 에서 켜야 한다. 안정화 전에는 쓰지 않는다 |
| `-unity-curvature` | 6.7 실험 | 곡면 UI, 런타임용 |

**없는 것 (대안)**

| CSS | 대안 |
|---|---|
| `box-shadow` | `filter: drop-shadow()`(6.6+) 또는 9-slice 그림자 이미지 |
| `gap`(6.7 미만) | 자식 `margin-right` / `margin-bottom`, 마지막 자식은 C# 또는 `--last` 클래스로 0 |
| `:focus-within` | C# `FocusInEvent` / `FocusOutEvent` 로 클래스 토글 |
| `:nth-child`, `:first-child`, `:last-child` | 생성할 때 C# 에서 `--odd` `--first` `--last` 클래스 부여 |
| `::before` / `::after` | UXML 에 실제 자식 요소 추가 |
| `calc()`, 변수 연산 | 테마에서 계산된 값을 변수 하나로 정의 |
| `em` `rem` `vw` `vh` | `px` 와 `%`, 배율은 루트 `scale` 또는 변수로 |
| `text-transform`, `line-height` | 없음. 글자 간격은 `letter-spacing`, 줄 간격은 `-unity-paragraph-spacing` 으로 일부 대체 |
| `grid`(안정판) | flex 중첩 또는 `MultiColumnListView` |

## 7. 작업 순서

1. **화면을 영역으로 나눈다.** 기능별, 컴포넌트별로 나누고 재사용될 것을 표시한다.
2. **재사용 컴포넌트부터 만든다.** `Components/<이름>/` 에 uxml, uss, cs 한 벌.
3. **화면을 조립한다.** 화면 UXML 에서 컴포넌트를 커스텀 태그나 `Instance` 로 배치한다.
4. **데이터를 바인딩으로 잇는다.** Inspector 는 `binding-path`, 목록은 `ListView` 의 `bindItem`.
5. **스타일을 변수로 정리한다.** 하드코딩 값이 있으면 테마 변수로 올린다.
6. **다크·라이트 둘 다 확인한다.**

## 8. 하지 말 것

| 안 좋은 모양 | 이렇게 한다 |
|---|---|
| `OnGUI` / `OnInspectorGUI` 에 `EditorGUILayout` 호출을 쌓기 | `CreateGUI` / `CreateInspectorGUI` 에서 UXML 을 읽어 `VisualElement` 로 |
| `EditorWindow` 안에서 `new Label()`, `new Button()` 수십 개를 조립 | UXML 로 옮기고 C# 은 `Q<>` 와 이벤트 연결만 |
| 한 UXML 에 툴바·목록·상세·설정을 전부 | 영역마다 UXML + USS + C# 로 분리 |
| `label.style.color = Color.red` | USS 클래스 `--error` 를 만들고 `AddToClassList` |
| 같은 버튼 모양을 두 화면 USS 에 복사 | `Components/` 로 뽑거나 `Common.uss` 로 올리기 |
| USS 에 `#3871e1`, `8px` 직접 기입 | `var(--color-accent)`, `var(--space-2)` |
| UXML 에 `style="margin: 4px"` | USS 클래스로 |
| `AssetDatabase.LoadAssetAtPath("Assets/...")` 를 파일마다 적기 | `UiAssets` 헬퍼 한 곳 |
| 컴포넌트가 `parent.parent.Q(...)` 로 바깥 구조 접근 | 이벤트로 부모에게 알리기 |
| 목록을 `foreach` 로 돌며 `Add(new Row())` | `ListView` 의 `makeItem` / `bindItem` |
| Inspector 에서 `serializedObject.FindProperty(...).intValue` 를 읽어 Label 에 써 넣기 | `binding-path` + `Bind()` |
| 클래스 이름 `.title`, `.row` 처럼 범용 이름 | `.card__title`, `.list__row` 처럼 블록 이름을 붙이기 |
| CSS 에서 쓰던 속성을 확인 없이 USS 에 적기(`box-shadow`, `:nth-child`, `calc()`) | 6절 지원 범위 표에서 확인하고 대안으로 |
| 프로젝트 최소 버전보다 늦게 생긴 USS 기능(`gap`, `backdrop-filter`)을 쓰기 | 최소 버전을 정하고 그 안에서만. 필요하면 버전을 올리는 결정을 먼저 |
| `width`, `left` 에 `transition` 걸기 | `translate` `scale` `opacity` 색에만 |

## 9. 마무리 점검

- [ ] UI 코드에 IMGUI(`OnGUI`, `OnInspectorGUI`, `EditorGUILayout`, `IMGUIContainer`)가 없다
- [ ] 모든 화면·컴포넌트가 `.uxml` + `.uss` + `.cs` 로 나뉘어 있다
- [ ] UXML 에 인라인 `style` 이 없고, C# 에 `style.xxx` 대입이 없다(계산한 위치·크기 제외)
- [ ] 컴포넌트 USS 규칙 안에 hex 색과 간격 숫자가 직접 쓰이지 않고 변수를 쓴다
- [ ] 한 파일이 여러 기능을 맡지 않는다. 100줄 넘는 UXML, 200줄 넘는 클래스는 쪼갤 곳을 확인했다
- [ ] 두 곳 이상에서 쓰는 모양은 컴포넌트나 `Common.uss` 로 올렸다
- [ ] 에셋 경로는 `UiAssets` 한 곳에만 있고, 못 찾으면 예외가 난다
- [ ] `name` · 클래스 문자열이 C# 상수로 모여 있다
- [ ] Inspector·Drawer 의 값은 `binding-path` 로 잇는다
- [ ] 목록은 `ListView` 를 쓴다
- [ ] 쓴 USS 속성·의사 클래스·함수가 6절 지원 범위 표에 있고, 프로젝트 최소 Unity 버전에서 되는 것이다
- [ ] 전환은 `translate` `scale` `rotate` 색 `opacity` 에만 걸었다
- [ ] Editor 전용 asmdef 안에 있다
- [ ] 다크·라이트 테마 둘 다에서 확인했다
