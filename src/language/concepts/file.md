<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# `.slint` 文件

用户界面是用Slint语言编写的，并且以`.slint`扩展名保存在文件中。

每个`.slint`文件可以定义了一个或多个组件。这些组件声明了一组元素树。组件是Slint中组合的基础。
使用它们来构建你自己的可重用UI控件集。
你可以使用每个声明的组件在其名称下作为另一个组件的元素。

以下是组件和元素的例子：

```slint

component MyButton inherits Text {
    color: black;
    // ...
}

export component MyApp inherits Window {
    preferred-width: 200px;
    preferred-height: 100px;
    Rectangle {
        width: 200px;
        height: 100px;
        background: green;
    }
    MyButton {
        x:0;y:0;
        text: "hello";
    }
    MyButton {
        y:0;
        x: 50px;
        text: "world";
    }
}

```

在这个场景中，我们有两个组件：`MyButton` 和 `MyApp`。`MyApp` 使用了内置元素 `Window` 和 `Rectangle`。
此外，`MyApp` 还重用了 `MyButton` 组件作为两个独立的元素。

元素具有属性，你可以为这些属性分配值。在这里，我们将一个字符串常量 "hello" 分配给第一个 `MyButton` 的 `text` 属性。你也可以分配整个表达式。当这些属性所依赖的任何属性发生变化时，Slint 将重新计算这些表达式，从而使用户界面具有响应性。

你可以使用`:=`语法来命名元素。

```slint
component MyButton inherits Text {
    // ...
}

export component MyApp inherits Window {
    preferred-width: 200px;
    preferred-height: 100px;

    hello := MyButton {
        x:0;y:0;
        text: "hello";
    }
    world := MyButton {
        y:0;
        text: "world";
        x: 50px;
    }
}
```
命名必须是有效的 [标识符](../syntax/identifiers.md)。

某些元素也可以通过预定义的名称进行访问：

- `root` 指向组件的最外层元素。
- `self` 指向当前元素。
- `parent` 指向当前元素的父元素。

这些名称是保留的，你不能重新定义它们。
