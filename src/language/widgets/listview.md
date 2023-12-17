<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `ListView`

一个 `ListView` 就像一个 [`ScrollView`](#scrollview)，但是它应该有一个 `for` 元素，而且内容是自动在列表中布局的。
元素只有在可见时才会被实例化。

### 属性

与 [`ScrollView`](#scrollview) 相同

### 示例

```slint
import { ListView } from "std-widgets.slint";
export component Example inherits Window {
    width: 150px;
    height: 150px;
    ListView {
        width: 150px;
        height: 150px;
        for data in [
            { text: "Blue", color: #0000ff, bg: #eeeeee},
            { text: "Red", color: #ff0000, bg: #eeeeee},
            { text: "Green", color: #00ff00, bg: #eeeeee},
            { text: "Yellow", color: #ffff00, bg: #222222 },
            { text: "Black", color: #000000, bg: #eeeeee },
            { text: "White", color: #ffffff, bg: #222222 },
            { text: "Magenta", color: #ff00ff, bg: #eeeeee },
            { text: "Cyan", color: #00ffff, bg: #222222 },
        ] : Rectangle {
            height: 30px;
            background: data.bg;
            width: parent.width;
            Text {
                x: 0;
                text: data.text;
                color: data.color;
            }
        }
    }
}
```
