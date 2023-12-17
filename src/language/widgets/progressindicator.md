<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `ProgressIndicator`

`ProgressIndicator` 通知用户正在进行的操作的状态，例如从网络加载数据。

### 属性

-   **`indeterminate`**：（_in_ _bool_）：如果操作的进度不能通过值确定，则设置为 true（默认值：`false`）。
-   **`progress`**（_in_ _float_）：完成百分比，作为 0 到 1 之间的值。小于 0 或大于 1 的值被截断。

### 示例

```slint
import { ProgressIndicator } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    ProgressIndicator {
        width: parent.width;
        height: parent.height;
        progress: 50%;
    }
}
```
