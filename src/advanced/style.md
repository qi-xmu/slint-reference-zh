<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 组件风格

Slint 提供了各种[内置小部件](../language/widgets/widgets.md)，可以从 `"std-widgets.slint"` 导入。您可以通过选择样式来修改这些小部件的外观。

可用的样式包括：

| 样式名称 | 亮色变体 | 暗色变体 | 描述 |
|------------|--------------|--------------|------------|
| `fluent`   | `fluent-light`| `fluent-dark`| 这些变体属于**Fluent**样式，它基于[Fluent Design System](https://fluent2.microsoft.design/)。 |
| `material` | `material-light`| `material-dark`| 这些变体是**Material**样式的一部分，它遵循[Material Design](https://m3.material.io)。 |
| `cupertino`| `cupertino-light`| `cupertino-dark`| **Cupertino**变体模拟了 macOS 使用的样式。 |
| `qt`   | | | **Qt**样式使用[Qt](https://en.wikipedia.org/wiki/Qt_(software))来渲染小部件。此样式需要在系统上安装 Qt。 |
| `native` | | | 这是一个别名，取决于平台，它是`cupertino`在 macOS 上，在 Windows 上是`fluent`，在 Android 上是`material`，在 linux 上是`qt`，如果 Qt 可用，否则是`fluent`。 |

默认情况下，样式会自动适应系统的暗色或亮色设置。选择`-light`或`-dark`变体以覆盖系统设置并始终显示暗色或亮色。

小部件样式是在项目的编译时确定的。选择样式的方法取决于您如何使用 Slint。

如果未选择样式，则`native`是默认值。

## 使用 Rust 选择小部件样式：

您可以通过将`SLINT_STYLE`环境变量设置为所选样式的名称来在开始编译之前选择样式。

使用`slint_build` API 时，调用[`slint_build::compile_with_config()`](https://docs.rs/slint-build/newest/slint_build/fn.compile_with_config.html)函数。

使用`slint_interpeter` API 时，调用[`slint_interpreter::ComponentCompiler::set_style()`](https://docs.rs/slint-interpreter/newest/slint_interpreter/struct.ComponentCompiler.html#method.set_style)函数。

## 使用 C++ 选择小部件样式

定义一个`SLINT_STYLE`CMake 缓存变量，其中包含样式名称作为字符串。例如，可以在命令行上执行此操作：

```sh
cmake -DSLINT_STYLE="material" /path/to/source
```

## 使用`slint-viewer`预览设计

通过设置`SLINT_STYLE`环境变量或使用`--style`参数传递样式名称来选择样式：

```sh
slint-viewer --style material /path/to/design.slint
```

## 使用 Slint Visual Studio Code 扩展预览设计

首先打开 Visual Studio Code 设置编辑器：

-   在 Windows/Linux 上 - File > Preferences > Settings
-   在 macOS 上 - Code > Preferences > Settings

然后在 Extensions > Slint > Preview:Style 中输入样式名称

## 使用通用 LSP 进程预览设计

在启动进程之前，通过设置`SLINT_STYLE`环境变量来选择样式。
或者，如果您的 IDE 集成允许使用命令行参数，则可以使用`--style`指定样式。
