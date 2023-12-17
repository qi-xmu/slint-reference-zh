<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 回调函数

组件可以声明回调函数，用于将状态的变化通知给外部。通过像调用函数一样“调用”它们来调用回调函数。

您可以通过使用 `=>` 箭头语法声明处理程序来响应回调函数的调用。
内置的 `TouchArea` 元素声明了一个 `clicked` 回调函数，当用户触摸元素覆盖的矩形区域或使用鼠标单击该区域时，该回调函数将被调用。
在下面的示例中，通过声明处理程序并调用我们的自定义回调函数，将回调函数的调用转发到另一个自定义回调函数（`hello`）：

```slint,no-preview
export component Example inherits Rectangle {
    // declare a callback
    callback hello;

    area := TouchArea {
        // sets a handler with `=>`
        clicked => {
            // emit the callback
            root.hello()
        }
    }
}
```

可以向回调函数添加参数：

```slint,no-preview
export component Example inherits Rectangle {
    // declares a callback
    callback hello(int, string);
    hello(aa, bb) => { /* ... */ }
}
```

回调函数也可以返回值：

```slint,no-preview
export component Example inherits Rectangle {
    // declares a callback with a return value
    callback hello(int, int) -> int;
    hello(aa, bb) => { aa + bb }
}
```

## 别名

可以类似于双向绑定来声明回调函数别名：

```slint,no-preview
export component Example inherits Rectangle {
    callback clicked <=> area.clicked;
    area := TouchArea {}
}
```
