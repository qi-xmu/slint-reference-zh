<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->

# 字体处理

诸如 `Text` 和 `TextInput` 之类的元素可以呈现文本，并允许通过不同的属性自定义文本的外观。
以 `font-` 为前缀的属性，例如 `font-family`，`font-size` 和 `font-weight` 影响用于渲染到屏幕的字体的选择。
如果没有指定这些属性中的任何一个，则周围 `Window` 元素中的 `default-font-` 值将应用，例如 `default-font-family`。

所选用于渲染的字体会自动从系统中获取。您也可以在设计中包含自定义字体。
自定义字体必须是 TrueType 字体（`.ttf`），TrueType 字体集（`.ttc`）或 OpenType 字体（`.otf`）。
您可以在 .slint 文件中使用 `import` 语句选择自定义字体：`import "./my_custom_font.ttf"`。
这会指示 Slint 编译器包含字体，并使字体系列在 `font-family` 属性中全局可用。

例如：

```slint,ignore
import "./NotoSans-Regular.ttf";

export component Example inherits Window {
    default-font-family: "Noto Sans";

    Text {
        text: "Hello World";
    }
}
```
