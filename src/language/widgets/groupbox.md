<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->

## `GroupBox`

`GroupBox` 是一个容器，它将其子元素组合在一个共同的标题下。

### 属性

-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，组框无法被按下
-   **`title`** (_in_ _string_): 作为组框标题的文本。

### 示例

```slint
import { GroupBox , VerticalBox, CheckBox } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 100px;
    GroupBox {
        title: "Groceries";
        VerticalLayout {
            CheckBox { text: "Bread"; checked: true ;}
            CheckBox { text: "Fruits"; }
        }
    }
}
```
