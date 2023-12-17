<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->

## `Spinner`

`Spinner` 通知用户正在进行的操作的状态，例如从网络加载数据。它提供了与 [`ProgressIndicator`](./progressindicator.md) 相同的属性，但形状不同。

### 属性

-   **`indeterminate`**: (_in_ _bool_): 如果操作的进度不能通过值来确定，则设置为 true（默认值：`false`）。
-   **`progress`** (_in_ _float_): 完成百分比，作为 0 到 1 之间的值。小于 0 或大于 1 的值被截断。

### 示例

```slint
import { Spinner } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    Spinner {
        progress: 50%;
    }
}
```
