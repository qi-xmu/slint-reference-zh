<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 后端 & 渲染器

在 Slint 中，后端是封装与操作系统的交互的模块，特别是窗口子系统。可以将多个后端编译到 Slint 中，并在应用程序启动时为使用选择一个后端。您可以在没有内置后端的情况下配置 Slint，而是通过实现 Slint 的平台抽象和窗口适配器接口来开发自己的后端。

后端的选择如下：

1. 开发人员提供自己的后端并以编程方式设置它。
2. 否则，后端是通过`SLINT_BACKEND`环境变量的值选择的，如果设置了该变量。
3. 否则，按照以下顺序尝试初始化后端：
   1. qt
   2. winit
   3. linuxkms

下表提供了内置后端的概述。有关后端功能及其配置选项的更多信息，请参见各个子页面。

| 后端名称 | 描述                                                                                             | 默认内置   |
|--------------|---------------------------------------------------------------------------------------------------------|-----------------------|
| qt           | 使用 Qt 库进行窗口系统集成、渲染和本机小部件样式。          | 是 (如果安装了 Qt) |
| winit        | 使用 [winit](https://docs.rs/winit/latest/winit/) 库与窗口系统交互。 | 是                   |
| linuxkms     | Linux 的 KMS/DRI 基础设施用于渲染。不需要窗口系统或合成器。    | 否                    |

后端还负责选择渲染器。请参见[渲染器](#渲染器)部分。
通过将名称添加到`SLINT_BACKEND`环境变量中（用破折号分隔）来覆盖渲染器的选择。例如，如果要选择`winit`后端以及`software`渲染器，请设置`SLINT_BACKEND=winit-software`。类似地，`SLINT_BACKEND=linuxkms-skia`选择`linuxkms`后端，然后指示 LinuxKMS 后端使用 Skia 进行渲染。

```{toctree}
:hidden:
:maxdepth: 2

backend_qt.md
backend_winit.md
backend_linuxkms.md
```

## 渲染器

Slint 提供了不同的渲染器，这些渲染器使用不同的技术和库将元素场景转换为像素。Slint 根据您选择的后端以及在 Slint 编译时选择的功能来选择渲染器后端。

### Qt 渲染器

Qt 渲染器随 [Qt 后端](backend_qt.md)一起提供，并使用 QPainter 进行渲染：

- 软件渲染，无 GPU 加速。
- 仅在 Qt 后端中可用。

### 软件渲染器

- 在任何地方运行，高度可移植且轻量级。
- 软件渲染，无 GPU 加速。
- 支持部分渲染。
- 支持逐行渲染（仅 Rust）。
- 适用于微控制器。
- 尚未实现某些功能：
  * 不支持`Path`。
  * 不支持图像旋转或平滑缩放。
  * 不支持`drop-shadow-*`属性。
  * 不支持`clip: true`与`border-radius`的组合。
  * 没有圆形渐变。

- 文本渲染目前仅限于西文。
- 在 [Winit 后端](backend_winit.md)中可用。
- 公共 [Rust](slint-rust:platform/software_renderer/) 和 [C++](slint-cpp:api/classslint_1_1platform_1_1SoftwareRenderer) API。

### FemtoVG 渲染器

- 高度可移植。
- GPU 加速，需要 OpenGL。
- 文本和路径渲染质量有时不太理想。
- 在 [Winit 后端](backend_winit.md)和 [LinuxKMS 后端](backend_linuxkms.md)中可用。
- 公共 [Rust](slint-rust:platform/femtovg_renderer/) API。

### Skia 渲染器

- 使用 OpenGL、Metal、Vulkan 和 Direct3D 的复杂 GPU 加速。
- 与其他渲染器相比，磁盘占用空间较大。
- 在 [Winit 后端](backend_winit.md)和 [LinuxKMS 后端](backend_linuxkms.md)中可用。
- 公共 [C++](slint-cpp:api/classslint_1_1platform_1_1SkiaRenderer) API。

#### 故障排除

启用 Skia 渲染器时，您可能会遇到编译问题。以下部分跟踪我们所知道的问题以及如何解决它们。

* Windows 上的编译错误，其中包含有关多个源文件和未使用的链接器输入的消息

  您可能会看到包含此错误和来自 clang-cl 的警告的编译错误：
  ```
   clang-cl: error: cannot specify '/Foobj/src/fonts/fontmgr_win.SkFontMgr_indirect.obj' when compiling multiple source files
   clang-cl: warning: Hausmann/.cargo/registry/src/index.crates.io-6f17d22bba15001f/skia-bindings-0.66.0/skia: 'linker' input unused [-Wunused-command-line-argument]
  ```

  Skia 源代码在 Cargo 管理的路径中检出，Rust 包管理器。当该路径包含空格时，错误会发生。默认情况下在 `%HOMEPATH%\.cargo`，如果登录名包含空格，则包含空格。要解决此问题，请将 `CARGO_HOME` 环境变量设置为不包含空格的路径，例如 `c:\cargo_home`。

* 在启用硬件浮点支持的 ARMv7 上编译时出现编译错误。

  您可能会看到包含此消息的编译错误：

  ```
   Unable to generate bindings: ClangDiagnostic("/home/runner/work/slint/yocto-sdk/sysroots/cortexa15t2hf-neon-poky-linux-gnueabi/usr/include/gnu/stubs-32.h:7:11: fatal error: 'gnu/stubs-soft.h' file not found\n")
  ```

  Skia 构建在多个场合下调用 clang，并且对影响头文件查找的浮点 abi 的编译器标志（例如`-mfloat-abi=hard`）敏感。

  要解决此问题，请将 `BINDGEN_EXTRA_CLANG_ARGS` 环境变量设置为包含您的构建环境也传递给 C++ 编译器的相同标志。

  例如，如果您正在使用 Yocto SDK 进行构建，则可以在`OECORE_TUNE_CCARGS`环境变量中找到这些标志。