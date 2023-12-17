<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `StandardListView`

和`ListView`一样，但是有一个默认的委托，以及一个`model`属性，它是类型为[`StandardListViewItem`](../builtins/structs.md#standardlistviewitem)的模型。

### 属性

和[`ListView`](#listview)一样，另外还有：

-   **`current-item`** (_in-out_ _int_): 当前活动项的索引。-1表示没有选中任何项，这是默认值
-   **`model`** (_in_ _[`StandardListViewItem`](../builtins/structs.md#standardlistviewitem)_): 模型

### 函数

-   **`set-current-item(int)`**: 通过指定的索引设置当前项，并将其带入视图。

### 回调

-   **`current-item-changed(int)`**: 当当前项因用户修改而更改时发出
-   **`item-pointer-event(int, PointerEvent, Point)`**: 在任何鼠标指针事件上发出，类似于`TouchArea`。参数是与事件相关联的项目索引，`PointerEvent`本身以及列表视图中的鼠标位置。

### 示例

```slint
import { StandardListView } from "std-widgets.slint";
export component Example inherits Window {
    width: 150px;
    height: 150px;
    StandardListView {
        width: 150px;
        height: 150px;
        model: [ { text: "Blue"}, { text: "Red" }, { text: "Green" },
            { text: "Yellow" }, { text: "Black"}, { text: "White"},
            { text: "Magenta" }, { text: "Cyan" },
        ];
    }
}
```
