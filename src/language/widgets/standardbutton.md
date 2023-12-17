<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `StandardButton`

`StandardButton` 看起来像一个按钮，但是不是用 `text` 和 `icon` 自定义的，它可以使用一个预定义的 `kind`，文本和图标将取决于样式。

### 属性

-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，按钮无法被按下
-   **`has-focus`**: (_out_ _bool_): 当按钮有键盘焦点时设置为 true
-   **`kind`** (_in_ _enum [`StandardButtonKind`](../builtins/enums.md#standardbuttonkind)_): 按钮的类型，其中之一为 `ok` `cancel`, `apply`, `close`, `reset`, `help`, `yes`, `no,` `abort`, `retry` 或 `ignore`
-   **`pressed`**: (_out_ _bool_): 当按钮被按下时设置为 true。

### 回调

-   **`clicked()`**

### 示例

```slint
import { StandardButton, VerticalBox } from "std-widgets.slint";
export component Example inherits Window {
  VerticalBox {
    StandardButton { kind: ok; }
    StandardButton { kind: apply; }
    StandardButton { kind: cancel; }
  }
}
```
