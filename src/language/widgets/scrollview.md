<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `ScrollView`

`ScollView` 包含一个比视图大的视口，可以滚动。它有滚动条来交互。视口宽度和视口高度会自动计算以创建一个可滚动的视图，除非使用 for 循环来填充元素。在这种情况下，视口宽度和视口高度不会自动计算，必须手动设置才能实现滚动。在将来可能会添加使用 for 循环时自动计算视口宽度和视口高度的功能，这在问题 #407 中有记录。

### 属性

-   **`enabled`** (_in_ _bool_): 用于将框架呈现为禁用或启用，但不会更改小部件的行为。
-   **`has-focus`** (_in-out_ _bool_): 用于将框架呈现为聚焦或未聚焦，但不会更改小部件的行为。
-   **`viewport-width`** 和 **`viewport-height`** (_in-out_ _length_): 视口的 `width` 和 `length` 属性
-   **`viewport-x`** 和 **`viewport-y`** (_in-out_ _length_): 视口的 `x` 和 `y` 属性。通常这些是负数
-   **`visible-width`** 和 **`visible-height`** (_out_ _length_): ScrollView 的可见区域的大小（不包括滚动条）

### 示例

```slint
import { ScrollView } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 200px;
    ScrollView {
        width: 200px;
        height: 200px;
        viewport-width: 300px;
        viewport-height: 300px;
        Rectangle { width: 30px; height: 30px; x: 275px; y: 50px; background: blue; }
        Rectangle { width: 30px; height: 30px; x: 175px; y: 130px; background: red; }
        Rectangle { width: 30px; height: 30px; x: 25px; y: 210px; background: yellow; }
        Rectangle { width: 30px; height: 30px; x: 98px; y: 55px; background: orange; }
    }
}
```
