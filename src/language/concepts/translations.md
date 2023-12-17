<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 翻译

使用 Slint 的翻译基础设施，使您的应用程序可以用不同的语言使用。

完成以下步骤来翻译您的应用程序：

1. 识别所有需要翻译的用户可见字符串，并使用 `@tr()` 宏对其进行注释。
2. 使用 `slint-tr-extractor` 工具提取注释的字符串并生成 `.pot` 文件。
3. 使用第三方工具将字符串翻译成目标语言，作为 `.po` 文件。
4. 使用 gettext 的 `msgfmt` 工具将 `.po` 文件转换为运行时可加载的 `.mo` 文件。
5. 使用 Slint 的 API 根据用户的区域设置选择和加载 `.mo` 文件。
   此时，所有标记为翻译的字符串都将以目标语言呈现。

## 注释可翻译的字符串

在 `.slint` 文件中使用 `@tr` 宏标记字符串可翻译。此宏将负责翻译和格式化，通过替换 `{}` 占位符。

第一个参数必须是一个普通的字符串字面量，后面跟着参数：

```slint,no-preview
export component Example {
    property <string> name;
    Text {
        text: @tr("Hello, {}", name);
    }
}
```

### 格式化

`@tr` 宏将字符串中的每个 `{}` 占位符替换为标记为翻译的字符串中的相应参数。
也可以使用 `{0}`，`{1}` 等来重新排序参数。即使原始字符串没有，翻译者也可以使用有序占位符。

字面字符 `{` 和 `}` 可以通过在相同的字符之前添加相同的字符来包含在字符串中。
例如，`{` 字符用 `{{` 转义，`}` 字符用 `}}` 转义。

### 复数

当涉及到变量数量的文本的翻译取决于是否有单个元素或多个元素时，使用复数格式。

给定 `count` 和表示某些东西的计数的表达式，使用 `|` 和 `%` 符号来形成复数，如下所示：

`@tr("I have {n} item" | "I have {n} items" % count)`。

在格式字符串中使用 `{n}` 来访问 `%` 之后的表达式。

```slint,no-preview
export component Example inherits Text {
    in property <int> score;
    in property <int> name;
    text: @tr("Hello {0}, you have one point" | "Hello {0}, you have {n} point" % score, name);
}
```

### 上下文

通过使用 `"..." =>` 语法向 `@tr(...)` 宏添加上下文，来消除具有相同源文本但具有不同上下文含义的字符串的歧义。

使用上下文为翻译者提供附加上下文信息，以确保准确和上下文适当的翻译。

上下文必须是一个普通的字符串字面量，并且它出现在 `.pot` 文件中的 `msgctx` 中。如果未指定，则上下文默认为周围组件的名称。

```slint,no-preview
export component MenuItem {
    property <string> name : @tr("Name" => "Default Name"); // Default: `MenuItem` will be the context.
    property <string> tooltip : @tr("ToolTip" => "ToolTip for {}", name); // Specified: The context will be `ToolTip`.
}
```

## 提取可翻译的字符串

使用 `slint-tr-extractor` 工具从 `.slint` 文件生成 `.pot` 文件。
您可以像这样运行它：

```sh
find -name \*.slint | xargs slint-tr-extractor -o MY_PROJECT.pot
```

这将创建一个名为 `MY_PROJECT.pot` 的文件。将 MY_PROJECT 替换为您的实际项目名称。
要了解项目名称如何影响翻译查找，请参阅下面的各节。

`.pot` 文件是 [Gettext](https://www.gnu.org/software/gettext/) 模板文件。

## 翻译文字

通过从 `.pot` 文件创建一个 `.po` 文件来开始一个新的翻译。这两种文件格式是相同的。
您可以手动复制文件，也可以使用 Gettext 的 `msginit` 等工具来创建一个新的 `.po` 文件。

`.po` 和 `.pot` 文件是纯文本文件，您可以使用文本编辑器编辑它们。
我们建议使用专门的翻译工具来处理它们，例如以下工具：

- [poedit](https://poedit.net/)
- [OmegaT](https://omegat.org/)
- [Lokalize](https://userbase.kde.org/Lokalize)
- [Transifex](https://www.transifex.com/) (web interface)


## 将 `.po` 文件转换为 `.mo` 文件

人类可读的 `.po` 文件需要转换为机器友好的 `.mo` 文件，这是一种非常高效的读取方式。

使用 [Gettext](https://www.gnu.org/software/gettext/) 的 `msgfmt` 命令行工具将 `.po` 文件转换为 `.mo` 文件：

```sh
msgfmt translation.po -o translation.mo
```

## 在运行时选择和加载 `.mo` 文件

Slint 使用 [Gettext](https://www.gnu.org/software/gettext/) 库在运行时加载翻译。
Gettext 在以下位置查找翻译文件：
Gettext 期望翻译文件（称为消息目录）放置在以下目录层次结构中：

```
dir_name/locale/LC_MESSAGES/domain_name.mo
```

* `dir_name`：您可以自由选择的基本目录。
* `locale`：给定目标语言的用户区域设置的名称，例如 `fr` 表示法语，`de` 表示德语。
  通常使用操作系统设置的环境变量来确定区域设置。
* `domain_name`：根据您使用 Slint 的编程语言选择。

有关更多信息，请参阅 [Gettext 文档](https://www.gnu.org/software/gettext/manual/gettext.html#Locating-Catalogs)。

### 使用 Rust 选择和加载翻译

首先，在 `slint` crate 中启用 `gettext` 特性，以获得对翻译 API 的访问，并激活运行时翻译支持。

接下来，使用 `slint::init_translations!` 来指定 `.mo` 文件的基本位置。
这是上一节中的 `dir_name`。`.mo` 文件应该在相应的子目录中，它们的文件名 - `domain_name` - 必须与您的 `Cargo.toml` 中的包名匹配。
这通常与 crate 名称相同。

例如：

```rust
slint::init_translations!(concat!(env!("CARGO_MANIFEST_DIR"), "/lang/"));
```

假设您的 `Cargo.toml` 包含以下行，用户的区域设置为 `fr`：

```toml
[package]
name = "gallery"
```

使用这些设置，Slint 将在 `lang/fr/LC_MESSAGES/gallery.mo` 中查找 `gallery.mo`。

### 使用 C++ 选择和加载翻译

首先，在 `slint` 库中启用 `SLINT_FEATURE_GETTEXT` cmake 选项，以获得对翻译 API 的访问，并激活运行时翻译支持。

在使用 cmake 的 C++ 应用程序中，`domain_name` 是 CMake 目标名称。

接下来，使用标准 gettext 库将文本域绑定到路径。

为此，请在 CMakeLists.txt 中添加以下内容：

```cmake
find_package(Intl)
if(Intl_FOUND)
    target_compile_definitions(gallery PRIVATE HAVE_GETTEXT SRC_DIR="${CMAKE_CURRENT_SOURCE_DIR}")
    target_link_libraries(gallery PRIVATE Intl::Intl)
endif()
```

你可以在 `main.cpp` 中设置区域设置和文本域

```c++
#ifdef HAVE_GETTEXT
#    include <locale>
#    include <libintl.h>
#endif

int main()
{
#ifdef HAVE_GETTEXT
    bindtextdomain("my_application", SRC_DIR "/lang/");
    std::locale::global(std::locale(""));
#endif
    //...
}
```

假设您使用上述设置，用户的区域设置为 `fr`，Slint 将在 `lang/fr/LC_MESSAGES/gallery.mo` 中查找 `gallery.mo`。

## 使用 `slint-viewer` 预览翻译

使用 `slint-viewer` 预览 `.slint` 文件时，使用 `gettext` 功能启用 `slint-viewer`。

1. 在 `slint-viewer` 的 `Cargo.toml` 中启用 `gettext` 功能。
2. 使用 `--translation-domain` 和 `translation-dir` 命令行选项加载翻译，并根据当前区域设置显示它们。