<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 内置全局单例

## `Palette`

使用 `Palette` 来创建与所选样式（例如 fluent、cupertino、material 或 qt）的颜色匹配的自定义小部件。

### 属性

-   **`background`** (_out_ _brush_): 定义默认的背景画刷。如果没有更专业的背景画刷适用，请使用此画刷。
-   **`foreground`** (_out_ _brush_): 定义用于显示在 `background` 画刷上的内容的前景画刷。
-   **`alternate-background`** (_out_ _brush_): 定义用于例如文本输入控件或面板（如侧边栏）的备用背景画刷。
-   **`alternate-foreground`** (_out_ _brush_): 定义用于显示在 `alternate-background` 画刷上的内容的前景画刷。
-   **`control-background`** (_out_ _brush_): 定义控件（例如按钮、组合框等）的默认背景画刷。
-   **`control-foreground`** (_out_ _brush_): 定义用于显示在 `control-background` 画刷上的内容的前景画刷。
-   **`accent-background`** (_out_ _brush_): 定义用于突出显示的控件（例如主按钮）的背景画刷。
-   **`accent-foreground`** (_out_ _brush_): 定义用于显示在 `accent-background` 画刷上的内容的前景画刷。
-   **`selection-background`** (_out_ _brush_): 定义用于突出显示的控件（例如文本选择）的背景画刷。
-   **`selection-foreground`** (_out_ _brush_): 定义用于显示在 `selection-background` 画刷上的内容的前景画刷。
-   **`border`** (_out_ _brush_): 定义用于边框（例如分隔符和小部件边框）的画刷。

### 示例

```slint
import { Palette, HorizontalBox } from "std-widgets.slint";

export component MyCustomWidget {
    in property <string> text <=> label.text;

    Rectangle {
        background: Palette.control-background;

        HorizontalBox {
            label := Text {
                color: Palette.control-foreground;
            }
        }
    }
}
```

## `TextInputInterface`

`TextInputInterface.text-input-focused` 属性可用于查找 `TextInput` 元素是否具有焦点。
如果您正在实现自己的虚拟键盘，则此属性是虚拟键盘是否应显示或隐藏的指示器。

### 属性

-   **`text-input-focused`** (_bool_): 如果 `TextInput` 元素具有焦点，则为 true；否则为 false。

### 示例

```slint
import { LineEdit } from "std-widgets.slint";

component VKB {
    Rectangle { background: yellow; }
}

export component Example inherits Window {
    width: 200px;
    height: 100px;
    VerticalLayout {
        LineEdit {}
        FocusScope {}
        if TextInputInterface.text-input-focused: VKB {}
    }
}
```
