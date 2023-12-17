<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `Slider`

### 属性

-   **`enabled`**：（_in_ _bool_）：默认为 true。如果启用为 false，则无法与滑块交互。
-   **`has-focus`**：（_out_ _bool_）：当滑块当前具有焦点时设置为 true
-   **`value`**（_inout_ _float_）：值。
-   **`minimum`**（_in_ _float_）：最小值（默认值：0）
-   **`maximum`**（_in_ _float_）：最大值（默认值：100）
-   **`orientation`**（_in_ _enum [`Orientation`](../builtins/enums.md#orientation)_）：如果设置为 true，则以垂直方式显示滑块（默认值：水平）。

### 回调

-   **`changed(float)`**：值已更改

### 示例

```slint
import { Slider } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    Slider {
        width: parent.width;
        height: parent.height;
        value: 42;
    }
}
```
