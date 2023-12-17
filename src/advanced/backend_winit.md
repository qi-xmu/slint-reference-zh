<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# Winit 后端

Winit 后端使用 [winit](https://docs.rs/winit/latest/winit/) 库与窗口系统交互。

Winit 后端支持几乎所有相关的操作系统和窗口系统，包括 macOS、Windows、Linux（使用 Wayland 和 X11）以及通过 KMS 或专有驱动程序的直接全屏渲染。

Winit 后端支持不同的渲染器。可以通过`SLINT_BACKEND`环境变量显式选择它们以供使用。

| 渲染器名称 | 支持/需要的图形 API            | 选择渲染器的`SLINT_BACKEND`值 |
|---------------|---------------------------------------------|------------------------------------------|
| FemtoVG       | OpenGL                                      | `winit-femtovg`                          |
| Skia          | OpenGL, Metal, Direct3D, Software-rendering | `winit-skia`                             |
| Skia Software | 仅软件渲染的 Skia           | `winit-skia-software`                    |
| software      | 仅软件渲染，不需要 GPU         | `winit-software`                         |

## 配置选项

Winit 后端读取并解释以下环境变量：

| 名称               | 接受的值       | 描述                                                        |
|--------------------|-----------------|--------------------------------------------------------------------|
| `SLINT_FULLSCREEN` | 任何值       | 如果设置了此变量，则每个窗口都会以全屏模式显示。 |
