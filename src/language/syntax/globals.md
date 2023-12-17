<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 全局单例

使用 `global Name { /* .. properties or callbacks .. */ }` 声明全局单例，以便在整个项目中使用属性和回调。

例如，这对于常见的颜色调色板可能很有用：

```slint,no-preview
global Palette  {
    in-out property<color> primary: blue;
    in-out property<color> secondary: green;
}

export component Example inherits Rectangle {
    background: Palette.primary;
    border-color: Palette.secondary;
    border-width: 2px;
}
```

导出全局变量以使其从其他文件中可访问（请参见[模块](modules.md)）。从导出主应用程序组件的文件中导出全局变量，以使其在业务逻辑中的本机代码中可见。

```slint,ignore
export global Logic  {
    in-out property <int> the-value;
    pure callback magic-operation(int) -> int;
}
// ...
```

<details data-snippet-language="rust">
<summary>从 Rust 中使用</summary>

```rust
slint::slint!{
export global Logic {
    in-out property <int> the-value;
    pure callback magic-operation(int) -> int;
}

export component App inherits Window {
    // ...
}
}

fn main() {
    let app = App::new();
    app.global::<Logic>().on_magic_operation(|value| {
        eprintln!("magic operation input: {}", value);
        value * 2
    });
    app.global::<Logic>().set_the_value(42);
    // ...
}
```

</details>

<details data-snippet-language="cpp">
<summary>从 C++ 中使用</summary>

```cpp
#include "app.h"

fn main() {
    auto app = App::create();
    app->global<Logic>().on_magic_operation([](int value) -> int {
        return value * 2;
    });
    app->global<Logic>().set_the_value(42);
    // ...
}
```

</details>

可以使用双向绑定语法从全局变量重新公开回调或属性。

```slint,no-preview
global Logic  {
    in-out property <int> the-value;
    pure callback magic-operation(int) -> int;
}

component SomeComponent inherits Text {
    // use the global in any component
    text: "The magic value is:" + Logic.magic-operation(42);
}

export component MainWindow inherits Window {
    // re-expose the global properties such that the native code
    // can access or modify them
    in-out property the-value <=> Logic.the-value;
    pure callback magic-operation <=> Logic.magic-operation;

    SomeComponent {}
}
```
