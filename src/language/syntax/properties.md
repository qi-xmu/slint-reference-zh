<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 属性

所有元素都有属性。内置元素带有常见的属性，例如颜色或尺寸属性。您可以为它们分配值或整个[表达式](expressions.md)：

```slint,no-preview
export component Example inherits Window {
    // 简单表达式：以分号结尾
    width: 42px;
    // 或者代码块（不需要分号）
    height: { 42px }
}
```

属性的默认值是类型的默认值。例如，布尔属性默认为 `false`，`int` 属性为零，等等。

除了现有属性之外，还可以通过指定名称、类型和可选的默认值来定义额外的属性：

```slint,no-preview
export component Example {
    // 声明一个名为 `my-property` 的 int 类型的属性
    property<int> my-property;

    // 声明一个带有默认值的属性
    property<int> my-second-property: 42;
}
```

使用限定符对额外的属性进行注释，以指定如何读取和写入属性：

-   **`private`**（默认）：该属性只能从组件内部访问。
-   **`in`**：该属性是输入。它可以被组件的用户设置和修改，例如通过绑定或在回调中赋值。组件可以提供默认绑定，但不能通过赋值来覆盖它。
-   **`out`**：输出属性，只能由组件设置。对于组件的用户来说，它是只读的。
-   **`in-out`**：该属性可以被所有人读取和修改。

```slint,no-preview
export component Button {
    // 这是组件的用户设置的。
    in property <string> text;
    // 这个属性是组件的用户读取的。
    out property <bool> pressed;
    // 这个属性既可以被用户又可以被组件本身改变。
    in-out property <bool> checked;

    // 这个属性是组件内部的。
    private property <bool> has-mouse;
}
```

除非使用组件作为元素或通过业务逻辑的语言绑定，否则所有在组件顶层声明的属性都可以从外部访问。

## 绑定

当绑定表达式中访问的属性发生变化时，绑定表达式会自动重新计算。

在下面的示例中，当用户按下按钮时，按钮的文本会自动更改。增加 `counter` 属性会自动使绑定到 `text` 的表达式无效，并触发重新计算。

```slint
import { Button } from "std-widgets.slint";

export component Example inherits Window {
    preferred-width: 50px;
    preferred-height: 50px;
    Button {
        property <int> counter: 3;
        clicked => { self.counter += 3 }
        text: self.counter * 2;
    }
}
```

当查询属性时，重新计算会延迟发生。

在内部，当计算绑定时，会为任何访问的属性注册依赖项。当属性发生变化时，依赖项会被通知，所有依赖的绑定都会被标记为脏。

默认情况下，本机代码中的回调不依赖于任何属性，除非它们在本机代码中查询属性。

## 双向绑定

使用 `<=>` 语法在属性之间创建双向绑定。这些属性将被链接在一起，并始终包含相同的值。

`<=>` 的右边必须是相同类型的属性的引用。如果未指定属性类型，则双向绑定将被推断。

```slint,no-preview
export component Example  {
    in property<brush> rect-color <=> r.background;
    // It's allowed to omit the type to have it automatically inferred
    in property rect-color2 <=> r.background;
    r:= Rectangle {
        width: parent.width;
        height: parent.height;
        background: blue;
    }
}
```

## 相对长度

有时以相对百分比的形式表达长度属性之间的关系很方便。例如下面的内部蓝色矩形的大小是外部绿色窗口的一半：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    background: green;
    Rectangle {
        background: blue;
        width: parent.width * 50%;
        height: parent.height * 50%;
    }
}
```

以百分比的形式表达父属性的 `width` 或 `height` 的关系是常见的模式。为了方便起见，这种情况下存在一种简写语法：

-   属性是 `width` 或 `height`
-   绑定表达式计算为百分比。

如果满足这些条件，则不需要指定父属性，而是可以直接使用百分比。前面的示例如下所示：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    background: green;
    Rectangle {
        background: blue;
        width: 50%;
        height: 50%;
    }
}
```