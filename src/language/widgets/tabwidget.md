<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `TabWidget`

`TabWidget` 是一组选项卡的容器。它只能有 `Tab` 元素作为子元素，一次只能有一个选项卡可见。

### 属性

-   **`current-index`** (_in_ _int_): 当前可见选项卡的索引

### `Tab` 元素的属性

-   **`title`** (_in_ _string_): 标签上的文本

### 示例

```slint
import { TabWidget } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 200px;
    TabWidget {
        Tab {
            title: "First";
            Rectangle { background: orange; }
        }
        Tab {
            title: "Second";
            Rectangle { background: pink; }
        }
    }
}
```
