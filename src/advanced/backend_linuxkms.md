<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# LinuxKMS 后端

LinuxKMS 后端仅在 Linux 上运行，并消除了对 Wayland 或 X11 等窗口系统的需求。相反，它使用以下库和接口直接渲染到屏幕并对触摸、鼠标和键盘输入做出反应。

- OpenGL 通过 KSM/DRI。
- Vulkan 通过 Vulkan KHR 显示扩展。
- libinput/libudev 用于来自鼠标、触摸屏或键盘的输入事件处理。
- libseat 用于 GPU 和输入设备访问，而无需 root 访问权限。 (可选)

在编译时，使用 pkg-config 来确定以下所需系统库的位置：

| pkg-config 包名称 | Debian 系发行版上的包名称 |
|-------------------------|--------------------------------------|
| `gbm`                   | `libgbm-dev`                         |
| `xkbcommon`             | `libxkbcommon-dev`                   |
| `libudev`               | `libudev-dev`                        |
| `libseat`               | `libseat-dev`                        |

:::{note}
如果在目标系统上没有可用的`libseat`，则可以选择`backend-linuxkms`，而不是选择`backend-linuxkms-noseat`。LinuxKMS 后端的这个变体消除了对安装 libseat 的需要，但是需要以能够访问所有输入和 DRM/KMS 设备文件的用户身份运行应用程序；通常是 root 用户。
:::

LinuxKMS 后端支持不同的渲染器。可以通过`SLINT_BACKEND`环境变量显式选择它们以供使用。

| 渲染器名称 | 需要的图形 API | 选择渲染器的`SLINT_BACKEND`值         |
|---------------|------------------------|------------------------------------------|
| FemtoVG       | OpenGL ES 2.0          | `linuxkms-femtovg`                               |
| Skia          | OpenGL ES 2.0, Vulkan  | `linuxkms-skia-opengl` 或 `linuxkms-skia-vulkan` |

:::{note}
此后端仍处于试验阶段。后端尚未在不同设备上进行过多种测试，并且存在已知问题。
:::

## 使用 OpenGL 选择显示

FemtoVG 使用 OpenGL，而 Skia - 除非启用了 Vulkan - 也使用 OpenGL。Linux 的直接渲染管理器 (DRM) 子系统用于配置显示输出。Slint 默认选择第一个连接的显示器，并将其配置为其首选分辨率（如果可用）或其最高分辨率。设置`SLINT_DRM_OUTPUT`环境变量以选择特定显示器。要获取可用输出的列表，请将`SLINT_DRM_OUTPUT`设置为`list`。、

例如，在具有内置屏幕 (eDP-1) 和外部连接的显示器 (DP-3) 的笔记本电脑上，输出可能如下所示：

```
DRM Output List Requested:
eDP-1 (connected: true)
DP-1 (connected: false)
DP-2 (connected: false)
DP-3 (connected: true)
DP-4 (connected: false)
```

将`SLINT_DRM_OUTPUT`设置为`DP-3`将在第二个显示器上呈现。

## 使用 Vulkan 选择显示

当启用 Skia 的 Vulkan 功能时，Skia 将尝试使用 Vulkan 的 KHR 显示扩展直接渲染到连接的屏幕。Slint 默认选择第一个连接的显示器，并将其配置为其首选分辨率（如果可用）或其最高分辨率。设置`SLINT_VULKAN_DISPLAY`环境变量以选择特定显示器。要获取可用输出的列表，请将`SLINT_VULKAN_DISPLAY`设置为`list`。例如，输出可能如下所示：

```
Vulkan Display List Requested:
Index: 0 Name: monitor
Index: 1 Name: monitor
```

将`SLINT_VULKAN_DISPLAY`设置为`1`将在第二个显示器上呈现。

要选择特定分辨率和刷新率（模式），请设置`SLINT_VULKAN_MODE`变量。将其设置为`list`以获取可用模式的列表。例如，输出可能如下所示：

```
Vulkan Mode List Requested:
Index: 0 Width: 3840 Height: 2160 Refresh Rate: 60
Index: 1 Width: 3840 Height: 2160 Refresh Rate: 59
Index: 2 Width: 3840 Height: 2160 Refresh Rate: 50
Index: 3 Width: 3840 Height: 2160 Refresh Rate: 30
Index: 4 Width: 3840 Height: 2160 Refresh Rate: 29
Index: 5 Width: 2560 Height: 1440 Refresh Rate: 59
Index: 6 Width: 1920 Height: 1080 Refresh Rate: 60
Index: 7 Width: 1920 Height: 1080 Refresh Rate: 59
Index: 8 Width: 1920 Height: 1080 Refresh Rate: 50
Index: 9 Width: 1680 Height: 1050 Refresh Rate: 59
...
```

要选择 1920x1080@60 模式，请将`SLINT_VULKAN_MODE`设置为`6`。

## 配置键盘

默认情况下，假定键盘布局和型号为美国型号和布局。设置以下环境变量以配置对不同键盘的支持：

- `XKB_DEFAULT_LAYOUT`：逗号分隔的布局 (语言) 列表，每个布局一个。有关接受的语言代码的列表，请参见[xkeyboard-config(7)](https://manpages.debian.org/testing/xkb-data/xkeyboard-config.7.en.html) 中的布局部分。
- `XKB_DEFAULT_MODEL`：用于解释键的键盘型号代码。有关接受的型号代码的列表，请参见[xkeyboard-config(7)](https://manpages.debian.org/testing/xkb-data/xkeyboard-config.7.en.html) 中的型号部分。
- `XKB_DEFAULT_VARIANT`：逗号分隔的变体列表，每个布局一个，用于配置特定于布局的变体。有关接受的变体代码的列表，请参见[xkeyboard-config(7)](https://manpages.debian.org/testing/xkb-data/xkeyboard-config.7.en.html) 中的布局部分中括号中的值。
- `XKB_DEFAULT_OPTIONS`：逗号分隔的选项列表，用于配置与布局无关的键组合。有关接受的选项代码的列表，请参见[xkeyboard-config(7)](https://manpages.debian.org/testing/xkb-data/xkeyboard-config.7.en.html) 中的选项部分。

