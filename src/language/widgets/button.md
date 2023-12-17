<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `Button`

一个简单的按钮。常见的按钮类型也可以用 [`StandardButton`](#standardbutton) 创建。

### 属性

-   **`checkable`** (_in_ _bool_): 显示按钮是否可以被选中。这使得 `checked` 属性可能变为 true。
-   **`checked`** (_inout_ _bool_): 显示按钮是否被选中。需要 `checkable` 为 true 才能工作。
-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，按钮无法被按下
-   **`has-focus`**: (_out_ _bool_): 当按钮有键盘焦点时设置为 true。
-   **`icon`** (_in_ _image_): 在按钮中显示的图像。请注意，并非所有样式都支持绘制图标。
-   **`pressed`**: (_out_ _bool_): 当按钮被按下时设置为 true。
-   **`text`** (_in_ _string_): 按钮中的文本。
-   **`primary`** (_in_ _bool_): 如果设置为 true，则使用主要强调色显示按钮（默认值：false）。
-   **`colorize-icon`** (_in_ _bool_): 如果设置为 true，则图标将被着色为与 Button 的文本颜色相同的颜色。 (默认值：false)

### 回调

-   **`clicked()`**

### 示例

```slint
import { Button, VerticalBox } from "std-widgets.slint";
export component Example inherits Window {
    VerticalBox {
        Button {
            text: "Click Me";
            clicked => { self.text = "Clicked"; }
        }
    }
}
```

