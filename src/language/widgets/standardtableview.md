<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->

## `StandardTableView`

`StandardTableView` 表示一个带有列和行的数据表。单元格在一个模型中组织，其中每一行都是一个 [`StandardListViewItem`](../builtins/structs.md#standardlistviewitem) 的模型。

### 属性

与 [`ListView`](#listview) 相同，另外还有：

-   **`current-sort-column`** (_out_ _int_): 表示排序的列。-1表示没有列被排序。
-   **`columns`** (_in-out_ _\[[`TableColumn`](../builtins/structs.md#tablecolumn)\]_): 定义表列的模型。
-   **`rows`** (_\[\[[`StandardListViewItem`](../builtins/structs.md#standardlistviewitem)\]\]_): 定义表行的模型。
-   **`current-row`** (_in-out_ _int_): 当前活动行的索引。-1表示没有选中任何行，这是默认值。

### 回调

-   **`sort-ascending(int)`**: 如果模型应该按升序按给定列排序，则发出。
-   **`sort-descending(int)`**: 如果模型应该按降序按给定列排序，则发出。
-   **`row-pointer-event(int, PointerEvent, Point)`**: 在任何鼠标指针事件上发出，类似于`TouchArea`。参数是与事件相关联的行索引，`PointerEvent`本身以及表视图中的鼠标位置。
-   **`current-row-changed(int)`**: 当前行因用户修改而更改时发出

### 函数

-   **`set-current-row(int)`**: 通过索引设置当前行，并将其带入视图。

### 示例

```slint
import { StandardTableView } from "std-widgets.slint";
export component Example inherits Window {
    width: 230px;
    height: 200px;
    StandardTableView {
        width: 230px;
        height: 200px;
        columns: [
            { title: "Header 1" },
            { title: "Header 2" },
        ];
        rows: [
            [
                { text: "Item 1" }, { text: "Item 2" },
            ],
            [
                { text: "Item 1" }, { text: "Item 2" },
            ],
            [
                { text: "Item 1" }, { text: "Item 2" },
            ]
        ];
    }
}
```
