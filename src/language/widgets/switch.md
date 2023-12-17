<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->

## `Switch`

`Switch` 是一个物理开关的表示，允许用户打开或关闭某些东西。如果您希望用户选择或取消选择值，请考虑使用 `CheckBox`。

### 属性

-   **`checked`**: (_inout_ _bool_): 开关是否被选中（默认值：false）。
-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，开关无法被按下（默认值：true）
-   **`has-focus`**: (_out_ _bool_): 当开关有键盘焦点时设置为 true（默认值：false）。
-   **`text`** (_in_ _string_): 开关旁边的文本。

### 回调

-   **`toggled()`**: 开关值已更改

### 示例

```slint
import { Switch } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    Switch {
        width: parent.width;
        height: parent.height;
        text: "Hello World";
    }
}
```
