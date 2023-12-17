<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `LineEdit`

一个用于输入单行文本的小部件。有关处理多行文本的小部件，请参见 [`TextEdit`](#textedit)。

### 属性

-   **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，不能输入任何内容，选择文本仍然可用，以及以编程方式编辑文本（默认值：`false`）
-   **`font-size`** (_in_ _length_): 输入文本的字体大小
-   **`has-focus`**: (_out_ _bool_): 当前焦点在行编辑器上时设置为 true
-   **`horizontal-alignment`** (_in_ _enum [`TextHorizontalAlignment`](../builtins/enums.md#texthorizontalalignment)_): 文本的水平对齐方式。
-   **`input-type`** (_in_ _enum [`InputType`](../builtins/enums.md#inputtype)_): 允许特殊输入查看属性的方式，例如密码字段（默认值：`text`）。
-   **`placeholder-text`**: (_in_ _string_): 在编辑字段中没有文本时显示的占位符文本
-   **`read-only`** (_in_ _bool_): 当设置为 true 时，禁用通过键盘和鼠标编辑文本，但
-   **`text`** (_in-out_ _string_): 正在编辑的文本

### 函数

-   **`focus()`** 调用此函数以聚焦 LineEdit 并使其接收未来的键盘事件。
-   **`select-all()`** 选择所有文本。
-   **`clear-selection()`** 清除选择。
-   **`copy()`** 将选定的文本复制到剪贴板。
-   **`cut()`** 将选定的文本复制到剪贴板并从可编辑区域中删除它。
-   **`paste()`** 将剪贴板的文本内容粘贴到光标位置。

### 回调

-   **`accepted(string)`**: 按下了回车键
-   **`edited(string)`**: 当用户修改文本时发出文本已更改

### 示例

```slint
import { LineEdit } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 25px;
    LineEdit {
        font-size: 14px;
        width: parent.width;
        height: parent.height;
        placeholder-text: "Enter text here";
    }
}
```
