<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
<!-- cSpell: ignore dgettext ngettext xgettext -->
# 自定义控件介绍

## 一个可点击的按钮

```slint,no-auto-preview
import { VerticalBox, Button } from "std-widgets.slint";
export component Recipe inherits Window {
    in-out property <int> counter: 0;
    VerticalBox {
        button := Button {
            text: "Button, pressed " + root.counter + " times";
            clicked => {
                root.counter += 1;
            }
        }
    }
}
```

在第一个示例中，您可以看到 Slint 语言的基础知识：

- 我们使用 `import` 语句从标准库中导入 `VerticalBox` 布局和 `Button` 小部件。此语句可以导入小部件或您在不同文件中声明的自定义组件。您不需要导入内置元素，例如 `Window` 或 `Rectangle`。
- 我们使用 `component` 关键字声明 `Recipe` 组件。`Recipe` 继承自 `Window`，并具有元素：一个布局（`VerticalBox`）和一个按钮。
- 您可以使用名称后跟一对大括号（可选内容）来实例化元素。您可以使用 `:=` 为特定元素分配名称。
- 元素具有属性。使用 `:` 来设置属性值。在这里，我们分配了一个绑定，该绑定通过连接一些字符串文字和 `counter` 属性来计算字符串，并将 `counter` 属性分配给 `Button` 的 `text` 属性。
- 您可以使用 `property <...>` 为任何元素声明自定义属性。属性需要具有类型，可以具有默认值和访问说明符。访问说明符（例如 `private`、`in`、`out` 或 `in-out`）定义了外部元素如何与该属性交互。`Private` 是默认值，并阻止任何外部元素访问该属性。在此示例中，`counter` 属性是自定义的。
- 元素还可以具有回调。在这种情况下，我们使用 `=> { ... }` 将回调处理程序分配给 `button` 的 `clicked` 回调。
- 如果绑定依赖的任何属性发生更改，则属性绑定会自动重新计算。按钮的 `text` 绑定在 `counter` 更改时自动重新计算。

## 在本机代码中响应按钮点击

此示例使用本机代码增加 `counter`：

```slint,no-preview
import { VerticalBox, Button } from "std-widgets.slint";
export component Recipe inherits Window {
    in-out property <int> counter: 0;
    callback button-pressed <=> button.clicked;
    VerticalBox {
        button := Button {
            text: "Button, pressed " + root.counter + " times";
        }
    }
}
```

`<=>` 语法将两个回调绑定在一起。在这里，新的 `button-pressed` 回调绑定到 `button.clicked`。

主组件的根元素将所有非 `private` 属性和回调公开给本机代码。

在 Slint 中，`-` 和 `_` 在所有标识符中都是等效的。这在本机代码中是不同的：大多数编程语言禁止在标识符中使用 `-`，因此 `-` 会替换为 `_`。

<details data-snippet-language="rust">
<summary>Rust 代码</summary>

由于技术原因，此示例在 `slint!` 宏中使用 `import {Recipe}`。在实际代码中，您可以将整个 Slint 代码放在 `slint!` 宏中，或者与构建脚本一起使用外部 `.slint` 文件。

```rust,no_run
slint::slint!(import { Recipe } from "docs/reference/src/recipes/button_native.slint";);

fn main() {
    let recipe = Recipe::new().unwrap();
    let recipe_weak = recipe.as_weak();
    recipe.on_button_pressed(move || {
        let recipe = recipe_weak.upgrade().unwrap();
        let mut value = recipe.get_counter();
        value = value + 1;
        recipe.set_counter(value);
    });
    recipe.run().unwrap();
}
```

Slint编译器为`Recipe`组件的根元素生成一个`struct Recipe`，其中包含每个可访问属性的getter（`get_counter`）和setter（`set_counter`）。它还为每个可访问回调生成一个函数，在这种情况下为`on_button_pressed`。

`Recipe`结构实现了[`slint::ComponentHandle`] trait。组件管理强引用和弱引用计数，类似于`Rc`。我们调用`as_weak`函数来获取对组件的弱引用，我们可以将其移动到回调中。

我们不能在这里使用强引用，因为这将形成一个循环：组件句柄拥有回调的所有权，回调本身拥有闭包的捕获变量的所有权。
</details>

<details data-snippet-language="cpp">
<summary>C++ 代码</summary>
在 C++ 中，你可以这样写

```cpp
#include "button_native.h"

int main(int argc, char **argv)
{
    auto recipe = Recipe::create();
    recipe->on_button_pressed([&]() {
        auto value = recipe->get_counter();
        value += 1;
        recipe->set_counter(value);
    });
    recipe->run();
}
```

CMake 集成会根据需要处理 Slint 编译器调用，该编译器将解析 `.slint` 文件并生成 `button_native.h` 头文件。

此头文件包含一个 `Recipe` 类，其中包含每个可访问属性的 getter 和 setter，以及一个函数来为 `Recipe` 中的每个可访问回调设置回调。在这种情况下，我们将有 `get_counter`、`set_counter` 来访问 `counter` 属性，以及 `on_button_pressed` 来设置回调。

</details>

## 使用属性绑定同步控件

```slint,no-auto-preview
import { VerticalBox, Slider } from "std-widgets.slint";
export component Recipe inherits Window {
    VerticalBox {
        slider := Slider {
            maximum: 100;
        }
        Text {
            text: "Value: \{round(slider.value)}";
        }
    }
}
```

此示例介绍了 `Slider` 小部件。

它还介绍了字符串文字中的插值：使用 `\{...}` 将花括号之间的代码的结果呈现为字符串。

# 动画示例

## 动画元素的位置

```slint,no-auto-preview
import { CheckBox } from "std-widgets.slint";
export component Recipe inherits Window {
    width: 200px;
    height: 100px;

    rect := Rectangle {
        x:0;
        y: 5px;
        width: 40px;
        height: 40px;
        background: blue;
        animate x {
            duration: 500ms;
            easing: ease-in-out;
        }
    }


    CheckBox {
        y: 25px;
        text: "Align rect to the right";
        toggled => {
            if (self.checked) {
                 rect.x = parent.width - rect.width;
            } else {
                 rect.x = 0px;
            }
        }
    }
}
```

布局自动定位元素。在此示例中，我们手动定位元素，使用 `x`、`y`、`width`、`height` 属性。

请注意 `animate x` 块，该块指定动画。每当属性更改时运行它：因为回调设置属性，或者因为其绑定值发生更改。

## 动画序列

```slint,no-auto-preview
import { CheckBox } from "std-widgets.slint";
export component Recipe inherits Window {
    width: 200px;
    height: 100px;

    rect := Rectangle {
        x:0;
        y: 5px;
        width: 40px;
        height: 40px;
        background: blue;
        animate x {
            duration: 500ms;
            easing: ease-in-out;
        }
        animate y {
            duration: 250ms;
            delay: 500ms;
            easing: ease-in;
        }
    }


    CheckBox {
        y: 25px;
        text: "Align rect bottom right";
        toggled => {
            if (self.checked) {
                 rect.x = parent.width - rect.width;
                 rect.y = parent.height - rect.height;
            } else {
                 rect.x = 0px;
                 rect.y = 0px;
            }
        }
    }
}
```

这个例子使用 `delay` 属性使一个动画在另一个动画之后运行。

# 状态示例

## 将属性值与状态关联

```slint,no-auto-preview
import { HorizontalBox, VerticalBox, Button } from "std-widgets.slint";

component Circle inherits Rectangle {
    width: 30px;
    height: 30px;
    border-radius: root.width / 2;
    animate x { duration: 250ms; easing: ease-in; }
    animate y { duration: 250ms; easing: ease-in-out; }
    animate background { duration: 250ms; }
}

export component Recipe inherits Window {
    states [
        left-aligned when b1.pressed: {
            circle1.x: 0px; circle1.y: 40px; circle1.background: green;
            circle2.x: 0px; circle2.y: 0px; circle2.background: blue;
        }
        right-aligned when b2.pressed: {
            circle1.x: 170px; circle1.y: 70px; circle1.background: green;
            circle2.x: 170px; circle2.y: 00px; circle2.background: blue;
        }
    ]

    VerticalBox {
        HorizontalBox {
            max-height: self.min-height;
            b1 := Button {
                text: "State 1";
            }
            b2 := Button {
                text: "State 2";
            }
        }
        Rectangle {
            background: root.background.darker(20%);
            width: 200px;
            height: 100px;

            circle1 := Circle { y:0; background: green; x: 85px; }
            circle2 := Circle { background: green; x: 85px; y: 40px; }
        }
    }
}
```

## 翻译

```slint,no-auto-preview
import { HorizontalBox, VerticalBox, Button } from "std-widgets.slint";

component Circle inherits Rectangle {
    width: 30px;
    height: 30px;
    border-radius: root.width / 2;
}

export component Recipe inherits Window {
    states [
        left-aligned when b1.pressed: {
            circle1.x: 0px; circle1.y: 40px;
            circle2.x: 0px; circle2.y: 0px;
            in {
                animate circle1.x, circle2.x { duration: 250ms; }
            }
            out {
                animate circle1.x, circle2.x { duration: 500ms; }
            }
        }
        right-aligned when !b1.pressed: {
            circle1.x: 170px; circle1.y: 70px;
            circle2.x: 170px; circle2.y: 00px;
        }
    ]

    VerticalBox {
        HorizontalBox {
            max-height: self.min-height;
            b1 := Button {
                text: "Press and hold to change state";
            }
        }
        Rectangle {
            background: root.background.darker(20%);
            width: 250px;
            height: 100px;

            circle1 := Circle { y:0; background: green; x: 85px; }
            circle2 := Circle { background: blue; x: 85px; y: 40px; }
        }
    }
}
```

# 布局示例

## 垂直布局

```slint,no-auto-preview
import { VerticalBox, Button } from "std-widgets.slint";
export component Recipe inherits Window {
    VerticalBox {
        Button { text: "First"; }
        Button { text: "Second"; }
        Button { text: "Third"; }
    }
}
```

## 水平布局

```slint,no-auto-preview
import { HorizontalBox, Button } from "std-widgets.slint";
export component Recipe inherits Window {
    HorizontalBox {
        Button { text: "First"; }
        Button { text: "Second"; }
        Button { text: "Third"; }
    }
}
```

## 网格

```slint,no-auto-preview
import { GridBox, Button, Slider } from "std-widgets.slint";
export component Recipe inherits Window {
    GridBox {
        Row {
            Button { text: "First"; }
            Button { text: "Second"; }
        }
        Row {
            Button { text: "Third"; }
            Button { text: "Fourth"; }
        }
        Row {
            Slider {
                colspan: 2;
            }
        }
    }
}
```

# 全局回调

## 从 Slint 调用全局注册的本机回调

此示例使用全局单例在本机代码中实现通用逻辑。
此单例还可以存储本机代码可以访问的属性。

注意：预览仅可视化 Slint 代码。它未连接到本机代码。

```slint,no-preview
import { HorizontalBox, VerticalBox, LineEdit } from "std-widgets.slint";

export global Logic  {
    pure callback to-upper-case(string) -> string;
    // You can collect other global properties here
}

export component Recipe inherits Window {
    VerticalBox {
        input := LineEdit {
            text: "Text to be transformed";
        }
        HorizontalBox {
            Text { text: "Transformed:"; }
            // Callback invoked in binding expression
            Text {
                text: {
                    Logic.to-upper-case(input.text);
                }
            }
        }
    }
}
```



<details  data-snippet-language="rust">
<summary>Rust 代码</summary>
在 Rust 中，您可以这样设置回调：

```rust
slint::slint!{
import { HorizontalBox, VerticalBox, LineEdit } from "std-widgets.slint";

export global Logic {
    pure callback to-upper-case(string) -> string;
    // You can collect other global properties here
}

export Recipe := Window {
    VerticalBox {
        input := LineEdit {
            text: "Text to be transformed";
        }
        HorizontalBox {
            Text { text: "Transformed:"; }
            // Callback invoked in binding expression
            Text {
                text: {
                    Logic.to-upper-case(input.text);
                }
            }
        }
    }
}
}

fn main() {
    let recipe = Recipe::new().unwrap();
    recipe.global::<Logic>().on_to_upper_case(|string| {
        string.as_str().to_uppercase().into()
    });
    // ...
}
```
</details>

<details  data-snippet-language="cpp">
<summary>C++ 代码</summary>
在 C++ 中，您可以这样设置回调：

```cpp
int main(int argc, char **argv)
{
    auto recipe = Recipe::create();
    recipe->global<Logic>().on_to_upper_case([](slint::SharedString str) -> slint::SharedString {
        std::string arg(str);
        std::transform(arg.begin(), arg.end(), arg.begin(), toupper);
        return slint::SharedString(arg);
    });
    // ...
}
```
</details>

# 自定义组件

## 自定义按钮

```slint,no-auto-preview
component Button inherits Rectangle {
    in-out property text <=> txt.text;
    callback clicked <=> touch.clicked;
    border-radius: root.height / 2;
    border-width: 1px;
    border-color: root.background.darker(25%);
    background: touch.pressed ? #6b8282 : touch.has-hover ? #6c616c :  #456;
    height: txt.preferred-height * 1.33;
    min-width: txt.preferred-width + 20px;
    txt := Text {
        x: (parent.width - self.width)/2 + (touch.pressed ? 2px : 0);
        y: (parent.height - self.height)/2 + (touch.pressed ? 1px : 0);
        color: touch.pressed ? #fff : #eee;
    }
    touch := TouchArea { }
}

export component Recipe inherits Window {
    VerticalLayout {
        alignment: start;
        Button { text: "Button"; }
    }
}
```

## 开关按钮

```slint,no-auto-preview
export component ToggleSwitch inherits Rectangle {
    callback toggled;
    in-out property <string> text;
    in-out property <bool> checked;
    in-out property<bool> enabled <=> touch-area.enabled;
    height: 20px;
    horizontal-stretch: 0;
    vertical-stretch: 0;

    HorizontalLayout {
        spacing: 8px;
        indicator := Rectangle {
            width: 40px;
            border-width: 1px;
            border-radius: root.height / 2;
            border-color: self.background.darker(25%);
            background: root.enabled ? (root.checked ? blue: white)  : white;
            animate background { duration: 100ms; }

            bubble := Rectangle {
                width: root.height - 8px;
                height: bubble.width;
                border-radius: bubble.height / 2;
                y: 4px;
                x: 4px + self.a * (indicator.width - bubble.width - 8px);
                property <float> a: root.checked ? 1 : 0;
                background: root.checked ? white : (root.enabled ? blue : gray);
                animate a, background { duration: 200ms; easing: ease;}
            }
        }

        Text {
            min-width: max(100px, self.preferred-width);
            text: root.text;
            vertical-alignment: center;
            color: root.enabled ? black : gray;
        }

    }

    touch-area := TouchArea {
        width: root.width;
        height: root.height;
        clicked => {
            if (root.enabled) {
                root.checked = !root.checked;
                root.toggled();
            }
        }
    }
}

export component Recipe inherits Window {
    VerticalLayout {
        alignment: start;
        ToggleSwitch { text: "Toggle me"; }
        ToggleSwitch { text: "Disabled"; enabled: false; }
    }
}
```

## 自定义滑块

`TouchArea` 覆盖整个小部件，因此您可以从其中的任何点拖动此滑块。

```slint,no-auto-preview
import { VerticalBox } from "std-widgets.slint";

export component MySlider inherits Rectangle {
    in-out property<float> maximum: 100;
    in-out property<float> minimum: 0;
    in-out property<float> value;

    min-height: 24px;
    min-width: 100px;
    horizontal-stretch: 1;
    vertical-stretch: 0;

    border-radius: root.height/2;
    background: touch.pressed ? #eee: #ddd;
    border-width: 1px;
    border-color: root.background.darker(25%);

    handle := Rectangle {
        width: self.height;
        height: parent.height;
        border-width: 3px;
        border-radius: self.height / 2;
        background: touch.pressed ? #f8f: touch.has-hover ? #66f : #0000ff;
        border-color: self.background.darker(15%);
        x: (root.width - handle.width) * (root.value - root.minimum)/(root.maximum - root.minimum);
    }
    touch := TouchArea {
        property <float> pressed-value;
        pointer-event(event) => {
            if (event.button == PointerEventButton.left && event.kind == PointerEventKind.down) {
                self.pressed-value = root.value;
            }
        }
        moved => {
            if (self.enabled && self.pressed) {
                root.value = max(root.minimum, min(root.maximum,
                    self.pressed-value + (touch.mouse-x - touch.pressed-x) * (root.maximum - root.minimum) / (root.width - handle.width)));

            }
        }
    }
}

export component Recipe inherits Window {
    VerticalBox {
        alignment: start;
        slider := MySlider {
            maximum: 100;
        }
        Text {
            text: "Value: \{round(slider.value)}";
        }
    }
}
```

这个例子显示了另一种实现，它具有可拖动的手柄：
只有在我们点击该手柄时，该手柄才会移动。
TouchArea 在手柄内部，并随手柄一起移动。

```slint,no-auto-preview
import { VerticalBox } from "std-widgets.slint";

export component MySlider inherits Rectangle {
    in-out property<float> maximum: 100;
    in-out property<float> minimum: 0;
    in-out property<float> value;

    min-height: 24px;
    min-width: 100px;
    horizontal-stretch: 1;
    vertical-stretch: 0;

    border-radius: root.height/2;
    background: touch.pressed ? #eee: #ddd;
    border-width: 1px;
    border-color: root.background.darker(25%);

    handle := Rectangle {
        width: self.height;
        height: parent.height;
        border-width: 3px;
        border-radius: self.height / 2;
        background: touch.pressed ? #f8f: touch.has-hover ? #66f : #0000ff;
        border-color: self.background.darker(15%);
        x: (root.width - handle.width) * (root.value - root.minimum)/(root.maximum - root.minimum);

        touch := TouchArea {
            moved => {
                if (self.enabled && self.pressed) {
                    root.value = max(root.minimum, min(root.maximum,
                        root.value + (self.mouse-x - self.pressed-x) * (root.maximum - root.minimum) / root.width));
                }
            }
        }
    }
}

export component Recipe inherits Window {
    VerticalBox {
        alignment: start;
        slider := MySlider {
            maximum: 100;
        }
        Text {
            text: "Value: \{round(slider.value)}";
        }
    }
}
```

## 自定义标签栏

使用这个示例作为基础，当您想要创建自己的自定义标签栏小部件时。

```slint,no-auto-preview
import { Button } from "std-widgets.slint";

export component Recipe inherits Window {
    preferred-height: 200px;
    in-out property <int> active-tab;
    VerticalLayout {
        tab_bar := HorizontalLayout {
            spacing: 3px;
            Button {
                text: "Red";
                clicked => { root.active-tab = 0; }
            }
            Button {
                text: "Blue";
                clicked => { root.active-tab = 1; }
            }
            Button {
                text: "Green";
                clicked => { root.active-tab = 2; }
            }
        }
        Rectangle {
            clip: true;
            Rectangle {
                background: red;
                x: root.active-tab == 0 ? 0 : root.active-tab < 0 ? - self.width - 1px : parent.width + 1px;
                animate x { duration: 125ms; easing: ease; }
            }
            Rectangle {
                background: blue;
                x: root.active-tab == 1 ? 0 : root.active-tab < 1 ? - self.width - 1px : parent.width + 1px;
                animate x { duration: 125ms; easing: ease; }
            }
            Rectangle {
                background: green;
                x: root.active-tab == 2 ? 0 : root.active-tab < 2 ? - self.width - 1px : parent.width + 1px;
                animate x { duration: 125ms; easing: ease; }
            }
        }
    }
}
```

## 自定义表格视图

Slint 提供了一个表格小部件，但是您也可以基于 `ListView` 做一些自定义。

```slint,no-auto-preview
import { VerticalBox, ListView } from "std-widgets.slint";

component TableView inherits Rectangle {
    in property <[string]> columns;
    in property <[[string]]> values;

    private property <length> e: self.width / root.columns.length;
    private property <[length]> column_sizes: [
        root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e,
        root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e,
        root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e, root.e,
    ];

    VerticalBox {
        padding: 5px;
        HorizontalLayout {
            padding: 5px; spacing: 5px;
            vertical-stretch: 0;
            for title[idx] in root.columns : HorizontalLayout {
                width: root.column_sizes[idx];
                Text { overflow: elide; text: title; }
                Rectangle {
                    width: 1px;
                    background: gray;
                    TouchArea {
                        width: 10px;
                        x: (parent.width - self.width) / 2;
                        property <length> cached;
                        pointer-event(event) => {
                            if (event.button == PointerEventButton.left && event.kind == PointerEventKind.down) {
                                self.cached = root.column_sizes[idx];
                            }
                        }
                        moved => {
                            if (self.pressed) {
                                root.column_sizes[idx] += (self.mouse-x - self.pressed-x);
                                if (root.column_sizes[idx] < 0) {
                                    root.column_sizes[idx] = 0;
                                }
                            }
                        }
                        mouse-cursor: ew-resize;
                    }
                }
            }
        }
        ListView {
            for r in root.values : HorizontalLayout {
                padding: 5px;
                spacing: 5px;
                for t[idx] in r : HorizontalLayout {
                    width: root.column_sizes[idx];
                    Text { overflow: elide; text: t; }
                }
            }
        }
    }
}

export component Example inherits Window {
   TableView {
       columns: ["Device", "Mount Point", "Total", "Free"];
       values: [
            ["/dev/sda1", "/", "255GB", "82.2GB"] ,
            ["/dev/sda2", "/tmp", "60.5GB", "44.5GB"] ,
            ["/dev/sdb1", "/home", "255GB", "32.2GB"] ,
       ];
   }
}
```

## 响应式用户界面断点

使用此示例实现响应式 SideBar，当父宽度小于给定的断点时，它会折叠。单击按钮时，SideBar 再次展开。使用蓝色分隔符调整容器的大小并测试响应式行为。

```slint,no-auto-preview
import { Button, StyleMetrics } from "std-widgets.slint";

export component SideBar inherits Rectangle {
    private property <bool> collapsed: root.reference-width < root.break-point;

    /// Defines the reference width to check `break-point`.
    in-out property <length> reference-width;

    /// If `reference-width` is less `break-point` the `SideBar` collapses.
    in-out property <length> break-point: 600px;

    /// Set the text of the expand button.
    in-out property <string> expand-button-text;

    width: 160px;

    container := Rectangle {
        private property <bool> expaned;

        width: parent.width;
        background: StyleMetrics.window-background.darker(0.2);

        VerticalLayout {
            padding: 2px;
            alignment: start;

            HorizontalLayout {
                alignment: start;

                if (root.collapsed) : Button {
                    checked: container.expaned;
                    text: root.expand-button-text;

                    clicked => {
                        container.expaned = !container.expaned;
                    }
                }
            }

            @children
        }

        states [
            expaned when container.expaned && root.collapsed : {
                width: 160px;

                in {
                    animate width { duration: 200ms; }
                }
                out {
                    animate width { duration: 200ms; }
                }
                in {
                        animate width { duration: 200ms; }
                }
                out {
                        animate width { duration: 200ms; }
                }
            }
        ]
    }

    states [
        collapsed when root.collapsed : {
            width: 62px;
        }
    ]
}

component Splitter inherits TouchArea {
    width: 4px;
    mouse-cursor: ew-resize;

    Rectangle {
        width: 100%;
        height: 100%;
        background: blue;
    }
}

export component SideBarTest inherits Window {
    preferred-width: 700px;
    min-height: 400px;
    background: gray;

    GridLayout {
        x: 0;
        width: splitter.x;

        Rectangle {
            height: 100%;
            col: 1;
            background: white;

            HorizontalLayout {
                padding: 8px;

                Text {
                    color: black;
                    text: "Content";
                }
            }
        }
        SideBar {
            col: 0;
            reference-width: parent.width;
            expand-button-text: "E";
        }
    }

    splitter := Splitter {
        x: root.width - self.width;
        height: 100%;

        moved => {
            self.x = min(root.width - self.width, max(400px, self.x + self.mouse-x - self.pressed-x));
        }
    }
}
```


<!--

more content:

## Input Events

### Keyboard Input

Receive key events, pass them to native code

### Mouse and Touch Input

### Flickable

-->
