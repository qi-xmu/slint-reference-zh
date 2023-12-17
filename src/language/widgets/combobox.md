<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `ComboBox`

一个按钮，当点击时打开一个弹出窗口来选择一个值。

### 属性

-   **`current-index`**: (_in-out_ _int_): 选中值的索引（如果没有选中值，则为-1）
-   **`current-value`**: (_in-out_ _string_): 当前选中的文本。
-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，组合框无法被按下。
-   **`has-focus`**: (_out_ _bool_): 当组合框有键盘焦点时设置为 true。
-   **`model`** (_in_ _\[string\]_): 可能值的列表

### 回调

-   **`selected(string)`**: 从组合框中选择了一个值。参数是当前选中的值。

### 示例

```slint
import { ComboBox } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 130px;
    ComboBox {
        y: 0px;
        width: self.preferred-width;
        height: self.preferred-height;
        model: ["first", "second", "third"];
        current-value: "first";
    }
}
```

