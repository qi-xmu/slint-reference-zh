<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 模块

在 `.slint` 文件中声明的组件可以作为其他 `.slint` 文件中的元素使用，通过导出和导入它们。

默认情况下，`.slint` 文件中声明的每个类型都是私有的。`export` 关键字可以改变这一点。

```slint,no-preview
component ButtonHelper inherits Rectangle {
    // ...
}

component Button inherits Rectangle {
    // ...
    ButtonHelper {
        // ...
    }
}

export { Button }
```

在上面的示例中，`Button` 可以从其他 `.slint` 文件中访问，但 `ButtonHelper` 不能。

还可以在不影响其内部使用的情况下，为导出的目的更改名称：

```slint,no-preview
component Button inherits Rectangle {
    // ...
}

export { Button as ColorButton }
```

在上面的示例中，`Button` 无法从外部访问，但可以使用名称 `ColorButton` 代替。

为方便起见，第三种导出组件的方法是直接声明它们为导出：

```slint,no-preview
export component Button inherits Rectangle {
    // ...
}
```

类似地，可以导入从其他文件导出的组件：

```slint,ignore
import { Button } from "./button.slint";

export component App inherits Rectangle {
    // ...
    Button {
        // ...
    }
}
```

如果两个文件都导出了相同名称的类型，则可以在导入时选择为其分配不同的名称：

```slint,ignore
import { Button } from "./button.slint";
import { Button as CoolButton } from "../other_theme/button.slint";

export component App inherits Rectangle {
    // ...
    CoolButton {} // from other_theme/button.slint
    Button {} // from button.slint
}
```

元素、全局变量和结构可以导出和导入。

还可以导出从其他文件导入的全局变量（参见[全局单例](globals.md)）：

```slint,ignore
import { Logic as MathLogic } from "math.slint";
export { MathLogic } // known as "MathLogic" when using native APIs to access globals
```

## 模块语法

导入类型支持以下语法：

```slint,ignore
import { export1 } from "module.slint";
import { export1, export2 } from "module.slint";
import { export1 as alias1 } from "module.slint";
import { export1, export2 as alias2, /* ... */ } from "module.slint";
```

导出类型支持以下语法：

```slint,ignore
// Export declarations
export component MyButton inherits Rectangle { /* ... */ }

// Export lists
component MySwitch inherits Rectangle { /* ... */ }
export { MySwitch };
export { MySwitch as Alias1, MyButton as Alias2 };

// Re-export all types from other module
export * from "other_module.slint";
```

## 组件库

将代码库拆分为单独的模块文件可以促进重用，并通过允许隐藏辅助组件来提高封装性。这在项目自己的目录结构中很有效。要在项目之间共享组件库，请使用组件库语法：

```slint,ignore
import { MySwitch } from "@mylibrary";
```

在上面的示例中，将从名为“mylibrary”的组件库导入 `MySwitch` 组件。此库的路径必须在编译时单独定义。使用以下方法之一来帮助 Slint 编译器将“mylibrary”解析为正确的磁盘路径：

* 当使用 Rust 和 `build.rs` 时，调用 [`with_library_paths`](slint-build-rust:struct.CompilerConfiguration#method.with_library_paths)
  以提供从库名称到路径的映射。
* 当从命令行调用 `slint-viewer` 时，为每个组件库传递 `-Lmylibrary=/path/to/my/library`。
* 当使用 VS code 扩展中的实时预览时，请使用 `Slint: Library Paths` 设置配置 Slint 扩展的库路径。
