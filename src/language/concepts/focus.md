<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 聚焦处理

某些元素，例如 `TextInput` 不仅接受鼠标/手指的输入，还接受来自（虚拟）键盘的按键事件。
为了让一个元素接收这些事件，它必须有焦点。这可以通过 `has-focus`（输出）属性来看到。

您可以通过调用 `focus()` 来手动激活元素上的焦点：

```slint
import { Button } from "std-widgets.slint";

export component App inherits Window {
    VerticalLayout {
        alignment: start;
        Button {
            text: "press me";
            clicked => { input.focus(); }
        }
        input := TextInput {
            text: "I am a text input field";
        }
    }
}
```

如果您的 `TextInput` 元素在组件中，那么您可以使用 `forward-focus` 属性来引用应该接收焦点的元素：

```slint
import { Button } from "std-widgets.slint";

component LabeledInput inherits GridLayout {
    forward-focus: input;
    Row {
        Text {
            text: "Input Label:";
        }
        input := TextInput {}
    }
}

export component App inherits Window {
    GridLayout {
        Button {
            text: "press me";
            clicked => { label.focus(); }
        }
        label := LabeledInput {
        }
    }
}
```

如果你使用 `forward-focus` 属性在一个 `Window` 上，那么指定的元素将在窗口第一次获得焦点时接收焦点 —— 它成为初始焦点元素。
