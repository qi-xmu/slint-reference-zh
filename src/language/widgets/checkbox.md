<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `CheckBox`

使用一个`CheckBox`来让用户选择或取消选择值，例如在具有多个选项的列表中。如果操作更像是打开或关闭某些东西，请考虑使用`Switch`元素。

### 属性

-   **`checked`**: (_inout_ _bool_): 复选框是否被选中（默认值：false）。
-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，复选框无法被按下（默认值：true）
-   **`has-focus`**: (_out_ _bool_): 当复选框有键盘焦点时设置为 true（默认值：false）。
-   **`text`** (_in_ _string_): 复选框旁边的文本。

### 回调

-   **`toggled()`**: 复选框值已更改

### 示例

```slint
import { CheckBox } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    CheckBox {
        width: parent.width;
        height: parent.height;
        text: "Hello World";
    }
}
```

