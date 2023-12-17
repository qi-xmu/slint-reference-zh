<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 容器组件

当创建组件时，有时候会影响到子元素的位置。例如，想象一个组件，它在用户放置在内部的元素上方绘制一个标签：

```slint,ignore
export component MyApp inherits Window {

    BoxWithLabel {
        Text {
            // ...
        }
    }

    // ...
}
```

你可以使用一个布局来实现这样的 `BoxWithLabel`。
默认情况下，像 `Text` 这样的子元素会成为 `BoxWithLabel` 的直接子元素，但是我们需要它们成为我们的布局的子元素。
为此，您可以使用 `@children` 表达式来改变默认的子元素放置方式：

```slint
component BoxWithLabel inherits GridLayout {
    Row {
        Text { text: "label text here"; }
    }
    Row {
        @children
    }
}

export component MyApp inherits Window {
    preferred-height: 100px;
    BoxWithLabel {
        Rectangle { background: blue; }
        Rectangle { background: yellow; }
    }
}
```
