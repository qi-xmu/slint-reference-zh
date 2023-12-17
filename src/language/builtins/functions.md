<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 内置函数

## `animation-tick() -> duration`

此函数返回一个单调递增的时间，可用于动画。
从绑定调用此函数将不断重新计算绑定。
它可以这样使用：`x: 1000px + sin(animation-tick() / 1s * 360deg) * 100px;` 或 `y: 20px * mod(animation-tick(), 2s) / 2s `

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    Rectangle {
        y:0;
        background: red;
        height: 50px;
        width: parent.width * mod(animation-tick(), 2s) / 2s;
    }

    Rectangle {
        background: blue;
        height: 50px;
        y: 50px;
        width: parent.width * abs(sin(360deg * animation-tick() / 3s));
    }
}
```

## `debug(...)`

调试函数可以将一个或多个值作为参数，打印它们，然后返回无。

