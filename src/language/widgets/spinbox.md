<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `SpinBox`

### 属性

-   **`enabled`**: (_in_ _bool_): 默认为 true。如果 enabled 为 false，则无法与 spinbox 交互。
-   **`has-focus`**: (_out_ _bool_): 当前 spinbox 拥有焦点时设置为 true
-   **`value`** (_in-out_ _int_): 值。
-   **`minimum`** (_in_ _int_): 最小值（默认值：0）。
-   **`maximum`** (_in_ _int_): 最大值（默认值：100）。

### 回调

-   **`edited(int)`**: 值已更改，因为用户修改了它。

### 示例

```slint
import { SpinBox } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    SpinBox {
        width: parent.width;
        height: parent.height;
        value: 42;
    }
}
```
