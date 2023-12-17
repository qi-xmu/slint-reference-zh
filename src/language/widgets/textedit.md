<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
## `TextEdit`

和[`LineEdit`](#lineedit)`类似，但可以用来输入多行文本。

_注意：_当前的实现只实现了很少的基本快捷键。更多的快捷键将在未来的版本中实现：<https://github.com/slint-ui/slint/issues/474>

### 属性

- **`font-size`** (_in_ _length_): 输入文本的字体大小
- **`text`** (_in-out_ _string_): 正在编辑的文本
- **`has-focus`**: (_in_out_ _bool_): 当前小部件是否有焦点
- **`enabled`**: (_in_ _bool_): 默认为 true。当为 false 时，不能输入任何内容
- **`read-only`** (_in_ _bool_): 当设置为 true 时，禁用通过键盘和鼠标编辑文本，但选择文本仍然可用以及以编程方式编辑文本（默认值：`false`）
- **`wrap`** (_in_ _enum [`TextWrap`](../builtins/enums.md#textwrap)_): 文本换行方式（默认值：`word-wrap`）。
- **`horizontal-alignment`** (_in_ _enum [`TextHorizontalAlignment`](../builtins/enums.md#texthorizontalalignment)_): 文本的水平对齐方式。

### 函数

- **`focus()`** 调用此函数以聚焦 TextEdit 并使其接收未来的键盘事件。
- **`select-all()`** 选择所有文本。
- **`clear-selection()`** 清除选择。
- **`copy()`** 将选定的文本复制到剪贴板。
- **`cut()`** 将选定的文本复制到剪贴板并从可编辑区域中删除它。
- **`paste()`** 将剪贴板的文本内容粘贴到光标位置。

### 回调

- **`edited(string)`**: 当用户修改文本时发出文本已更改

### 示例

```slint
import { TextEdit } from "std-widgets.slint";
export component Example inherits Window {
    width: 200px;
    height: 200px;
    TextEdit {
        font-size: 14px;
        width: parent.width;
        height: parent.height;
        text: "Lorem ipsum dolor sit amet,\n consectetur adipisici elit";
    }
}
```
