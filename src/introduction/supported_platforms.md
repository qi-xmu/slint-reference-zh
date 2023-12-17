<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 支持的平台

Slint 是一个可以在许多桌面、嵌入式平台和微控制器上运行的系统。

在下面描述的平台中，已经在部署环境中测试过。
对于开发环境，我们推荐使用最新的桌面操作系统和编译器。

如果你需要获取特定旧版本的支持，请与[SixtyFPS GmbH](https://slint.dev/contact)联系。

## 桌面平台

一般来说，Slint 可以在 Windows、macOS 和流行的 Linux 发行版上运行。
以下表格中，列出了我们专门测试过的版本。
总体目标是在 Slint 版本发布时，支持由供应商支持的操作系统。

### Windows

| 操作系统   | 架构   |
| ---------- | ------ |
| Windows 10 | x86-64 |
| Windows 11 | x86-64 |

### macOS

| 操作系统          | 架构            |
| ----------------- | --------------- |
| macOS 11 Big Sur  | x86-64, aarch64 |
| macOS 12 Monterey | x86-64, aarch64 |
| macOS 13 Ventura  | x86-64, aarch64 |

### Linux

Linux桌面发行版呈现出多样化的格局，Slint应该能够在任何这些发行版上运行，前提是Linux桌面发行版呈现出多样化的格局，Slint应该能够在任何这些发行版上运行，前提是它们使用的是Wayland或X-Windows、glibc和d-bus。如果某个Linux发行版提供了长期支持（Long Term Support，LTS），那么在Slint版本发布时，它应该能够在最新的LTS或更新的版本上运行。

## 嵌入式平台

Slint 是一个可以在多种嵌入式平台上运行的软件。一般来说，Slint 需要一个现代的 Linux 用户空间，并且需要有可用的 OpenGL ES 2.0（或更高版本）或 Vulkan 驱动程序。我们已经成功地在以下平台运行了 Slint：

 - Yocto基于的发行版。对于C++应用程序，请参阅[meta-slint](https://github.com/slint-ui/meta-slint)以获取配方。Rust应用程序与Yocto的Rust支持无缝配合使用。
 - BuildRoot 基础发行版。
 - [TorizonCore](https://www.torizon.io/torizoncore-os).

### 微处理器

Slint的平台抽象允许集成到任何基于Rust或C++的微控制器开发环境中。开发人员需要实现功能，以提供输入事件，例如触摸或键盘，并将由Slint渲染的像素显示到帧缓冲区或行缓冲区中。