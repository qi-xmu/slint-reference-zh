<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 内置元素

## 通用属性

### 几何

这些属性对所有可见项都有效：

-   **`width`** 和 **`height`** (_in_ _length_): 元素的大小。当设置时，这将覆盖默认大小。
-   **`x`** 和 **`y`** (_in_ _length_): 元素相对于其父元素的位置。
-   **`z`** (_in_ _float_): 允许指定与兄弟项堆叠的不同顺序。 (默认值：0)
-   **`absolute-position`** (_in_ _Point_): 元素在包含窗口中的位置。

### 布局

这些属性对所有可见项都有效，并且可以在布局中使用以指定约束：

-   **`col`**、**`row`**、**`colspan`**、**`rowspan`** (_in_ _int_): 参见 [`GridLayout`](#gridlayout)。
-   **`horizontal-stretch`** 和 **`vertical-stretch`** (_in-out_ _float_): 指定这些元素在布局中拉伸的相对空间量。当为 0 时，这意味着除非所有元素都为 0，否则不会拉伸元素。内置小部件的值为 0 或 1。
-   **`max-width`** 和 **`max-height`** (_in_ _length_): 元素的最大大小
-   **`min-width`** 和 **`min-height`** (_in_ _length_): 元素的最小大小
-   **`preferred-width`** 和 **`preferred-height`** (_in_ _length_): 元素的首选大小

### 杂项

-   **`cache-rendering-hint`** (_in_ _bool_): 当设置为 `true` 时，这会向渲染器提供一个提示，将元素及其所有子元素的内容缓存到一个中间缓存层中。对于很少更改的复杂子树，这可能会加快渲染速度，但代价是增加了内存消耗。并非所有渲染后端都支持此功能，因此这仅仅是一个提示。 (默认值：`false`)
-   **`dialog-button-role`** (_in_ _enum [`DialogButtonRole`](enums.md#dialogbuttonrole)_): 指定这是 `Dialog` 中的按钮。
-   **`opacity`** (_in_ _float_): 0 到 1 之间的值（或百分比），用于以透明方式绘制元素及其子元素。0 是完全透明（不可见），1 是完全不透明。透明度被应用于子元素的树，就好像它们首先被绘制到一个中间层中，然后整个层以此透明度渲染。 (默认值：1)
-   **`visible`** (_in_ _bool_): 当设置为 `false` 时，元素及其所有子元素都不会被绘制，也不会对鼠标输入做出反应 (默认值：`true`)

下面的示例演示了具有子元素的 `opacity` 属性。一个不透明度应用于红色矩形。由于绿色矩形是红色矩形的子元素，您可以看到它下面的渐变，但是您无法通过绿色矩形看到红色矩形。

```slint
export component Example inherits Window {
    width: 100px;
    height: 100px;
    background: @radial-gradient(circle, black, white, black, white);
    Rectangle {
        opacity: 0.5;
        background: red;
        border-color: #822;
        border-width: 5px;
        width: 50px; height: 50px;
        x: 10px; y: 10px;
        Rectangle {
            background: green;
            border-color: #050;
            border-width: 5px;
            width: 50px; height: 50px;
            x: 25px; y: 25px;
        }
    }
}
```

### 可访问性

使用以下 `accessible-` 属性使您的项与屏幕阅读器、点字终端和其他软件进行良好的交互，以使您的应用程序可访问。

-   **`accessible-role`** (_in_ _enum [`AccessibleRole`](enums.md#accessiblerole)_): 元素的角色。必须设置此属性才能使用任何其他可访问属性。它应该设置为一个常量值。 (默认值：大多数元素为 `none`，但 `text` 元素为 `text`)
-   **`accessible-checkable`** (_in_ _bool_): 元素是否可以被选中。
-   **`accessible-checked`** (_in_ _bool_): 元素是否被选中。这对应于复选框、单选按钮和其他小部件的“选中”状态。
-   **`accessible-description`** (_in_ _string_): 当前元素的描述。
-   **`accessible-has-focus`** (_in_ _bool_): 当前元素当前是否具有焦点。
-   **`accessible-label`** (_in_ _string_): 交互元素的标签。 (默认值：大多数元素为空，或者 `text` 属性的值为 Text 元素)
-   **`accessible-value-maximum`** (_in_ _float_): 项目的最大值。这用于例如旋转框。
-   **`accessible-value-minimum`** (_in_ _float_): 项目的最小值。
-   **`accessible-value-step`** (_in_ _float_) 当前值可以更改的最小增量或减量。这对应于滑块上的手柄可以拖动的步长。
-   **`accessible-value`** (_in_ _string_): 项目的当前值。

### 投影

为了实现图形效果，即在元素的框架下方显示阴影效果的视觉提升形状，可以设置以下 `drop-shadow` 属性：

-   **`drop-shadow-blur`** (_in_ _length_): 阴影的半径，也描述了阴影的模糊程度。忽略负值，零表示没有模糊。 (默认值：0)
-   **`drop-shadow-color`** (_in_ _color_): 用于阴影的基本颜色。通常，该颜色是渐变的起始颜色，渐变会淡入透明度。
-   **`drop-shadow-offset-x`** 和 **`drop-shadow-offset-y`** (_in_ _length_): 阴影与元素框架的水平和垂直距离。负值将阴影放置在元素的左侧/上方。

`drop-shadow` 效果支持 `Rectangle` 元素。

## `Dialog`

Dialog 类似于窗口，但它具有自动布局的按钮。

Dialog 应该有一个主元素作为子元素，该元素不是按钮。
对话框可以有任意数量的 `StandardButton` 小部件或其他具有 `dialog-button-role` 属性的按钮。
按钮将按照目标平台在运行时的顺序放置。

`StandardButton` 的 `kind` 属性和 `dialog-button-role` 属性需要设置为常量值，它不能是任意变量表达式。
不能有多个相同类型的 `StandardButton`。

对于没有显式回调处理程序的每个 `StandardButton`，都会自动添加一个回调 `<kind>_clicked`，因此可以从本机代码处理它：例如，如果有一个 `cancel` 类型的按钮，则会添加一个 `cancel_clicked` 回调。

### 属性

-   **`icon`** (_in_ _image_): 窗口图标，显示在标题栏或窗口管理器支持的任务栏上。
-   **`title`** (_in_ _string_): 窗口标题，显示在标题栏中。

### 示例

```slint
import { StandardButton, Button } from "std-widgets.slint";
export component Example inherits Dialog {
    Text {
      text: "This is a dialog box";
    }
    StandardButton { kind: ok; }
    StandardButton { kind: cancel; }
    Button {
      text: "More Info";
      dialog-button-role: action;
    }
}
```

## `Flickable`

`Flickable` 是一个低级元素，是可滚动小部件（例如 [`ScrollView`](../widgets/scrollview.md)）的基础。
当 `viewport-width` 或 `viewport-height` 大于父元素的 `width` 或 `height` 时，元素变为可滚动。
请注意，`Flickable` 不会创建滚动条。当未设置时，`viewport-width` 和 `viewport-height` 会根据 `Flickable` 的子元素自动计算。
当使用 `for` 循环来填充元素时，情况并非如此。
这是一个小Bug，跟踪在问题 [#407](https://github.com/slint-ui/slint/issues/407).
`Flickable` 的最大和首选大小基于视口。

当不在布局中时，其宽度或高度默认为未指定时父元素的 100%。

### 指针事件交互

如果 `Flickable` 的区域包含使用 `TouchArea` 来响应点击的元素，例如 `Button` 小部件，则使用以下算法来区分用户滚动或与 `TouchArea` 元素交互的意图：

1. 如果 `Flickable` 的 `interactive` 属性为 `false`，则所有事件都将转发到下面的元素。
2. 如果接收到与 `TouchArea` 交互的坐标的按下事件，则存储事件，并且任何后续的移动和释放事件都将按照以下方式处理：
   1. 如果 100ms 没有任何事件，则存储的按下事件将传递给 `TouchArea`。
   2. 如果在 100ms 之前收到释放事件，则存储的按下事件以及释放事件将立即传递给 `TouchArea`，并且算法将重置。
   3. 收到的任何移动事件都将在以下情况下在 `Flickable` 上启动滑动操作，如果满足以下所有条件：
        1. 在收到按下事件后 500ms 之前收到事件。
        2. 到按下事件的距离在我们允许移动的方向上超过 8 个逻辑像素。
      如果 `Flickable` 决定滑动，则在 `TouchArea` 之前发送到 `TouchArea` 的任何按下事件都将跟随退出事件。
      在接收移动事件的阶段，`Flickable` 将跟随坐标。

3. 如果按下、移动和释放事件的交互始于与 `TouchArea` 不相交的坐标，则在以下情况下 `Flickable` 将立即滑动指针移动事件，当到按下事件的坐标的欧几里得距离超过 8 个逻辑像素时。

### 属性

-   **`interactive`** (_in_ _bool_): 当为 true 时，可以通过单击并使用光标拖动来滚动视口。 (默认值：true)
-   **`viewport-height`**、**`viewport-width`** (_in_ _length_): 滚动元素的总大小。
-   **`viewport-x`**、**`viewport-y`** (_in_ _length_): 滚动元素相对于 `Flickable` 的位置。这通常是一个负值。

### 示例

```slint
export component Example inherits Window {
    width: 270px;
    height: 100px;

    Flickable {
        viewport-height: 300px;
        Text {
            x:0;
            y: 150px;
            text: "This is some text that you have to scroll to see";
        }
    }
}
```

## `FocusScope`

`FocusScope` 公开回调以拦截键盘事件。请注意，`FocusScope` 仅在它 `has-focus` 时才会调用它们。

[`KeyEvent`](structs.md#keyevent) 有一个 text 属性，它是输入的键的字符。
当按下非打印键时，字符将是控制字符，或者它将映射到私有 unicode 字符。这些不可打印的特殊字符的映射在 [`Key`](namespaces.md#key) 命名空间中可用。

### 属性

-   **`has-focus`** (_out_ _bool_): 当元素具有键盘焦点时为 `true`。
-   **`enabled`** (_in_ _bool_): 当为 true 时，`FocusScope` 将在单击时使自身成为焦点元素。如果不希望单击以获得焦点的行为，则将其设置为 false。
    类似地，禁用的 `FocusScope` 不会通过 tab 焦点遍历接受焦点。即使 `enabled` 设置为 false，父 `FocusScope` 仍将接收来自被拒绝的子 `FocusScope` 的键事件。
    (默认值：true)

### 函数

-   **`focus()`** 调用此函数将键盘焦点转移到此 `FocusScope`，以接收未来的 [`KeyEvent`](structs.md#keyevent)。

### 回调

-   **`key-pressed(KeyEvent) -> EventResult`**: 按下键时调用，参数是 [`KeyEvent`](structs.md#keyevent) 结构。返回的 [`EventResult`](enums.md#eventresult) 指示是否接受或忽略事件。被忽略的事件将转发到父元素。
-   **`key-released(KeyEvent) -> EventResult`**: 释放键时调用，参数是 [`KeyEvent`](structs.md#keyevent) 结构。返回的 [`EventResult`](enums.md#eventresult) 指示是否接受或忽略事件。被忽略的事件将转发到父元素。
-   **`focus-changed-event()`**: 当焦点在 `FocusScope` 上发生变化时调用。

### 示例

```slint
export component Example inherits Window {
    width: 100px;
    height: 100px;
    forward-focus: my-key-handler;
    my-key-handler := FocusScope {
        key-pressed(event) => {
            debug(event.text);
            if (event.modifiers.control) {
                debug("control was pressed during this event");
            }
            if (event.text == Key.Escape) {
                debug("Esc key was pressed")
            }
            accept
        }
    }
}
```

## `GridLayout`

`GridLayout` 将其子元素放置在网格中。`GridLayout` 为每个子元素添加属性：`col`、`row`、`colspan`、`rowspan`。
您可以使用 `col` 和 `row` 控制子元素的位置。
如果未指定 `col` 或 `row`，则会自动计算它们，以便该项位于上一项旁边，同一行中。
或者，可以将该项放在 `Row` 元素中。

### 属性

-   **`spacing`** (_in_ _length_): 网格中的项之间的距离
-   **`spacing-horizontal`** 和 **`spacing-vertical`** (_in_ _length_): 网格中的项之间的水平和垂直距离
-   **`padding`** (_in_ _length_): 网格周围的空间.
-   **`padding-left`**、**`padding-right`**、**`padding-top`** 和 **`padding-bottom`** (_in_ _length_): 设置这些属性将覆盖 `padding` 属性。

### 示例

这个例子使用 `Row` 元素。

```slint
export component Foo inherits Window {
    width: 200px;
    height: 200px;
    GridLayout {
        spacing: 5px;
        Row {
            Rectangle { background: red; }
            Rectangle { background: blue; }
        }
        Row {
            Rectangle { background: yellow; }
            Rectangle { background: green; }
        }
    }
}
```

这个例子使用 `col` 和 `row` 属性。

```slint
export component Foo inherits Window {
    width: 200px;
    height: 200px;
    GridLayout {
        spacing: 5px;
        Rectangle { background: red; col: 0; row: 0; }
        Rectangle { background: blue; col: 1; row: 0; }
        Rectangle { background: yellow; col: 0; row: 1; }
        Rectangle { background: green; col: 1; row: 1; }
    }
}
```

## `Image`

一个 `Image` 元素显示一个图像。图像可以是从文件加载的，也可以是内存中的图像。

### 属性

-   **`colorize`** (_in_ _brush_): 当设置时，图像被用作 alpha 蒙版，并以给定的颜色（或渐变）绘制。
-   **`image-fit`** (_in_ _enum [`ImageFit`](enums.md#imagefit)_): 指定如何将源图像适合到图像元素中。 (默认值：当 `Image` 元素是布局的一部分时为 `contain`，否则为 `fill`)
-   **`image-rendering`** (_in_ _enum [`ImageRendering`](enums.md#imagerendering)_): 指定如何缩放源图像。 (默认值：`smooth`)
-   **`rotation-angle`** (_in_ _angle_), **`rotation-origin-x`** (_in_ _length_), **`rotation-origin-y`** (_in_ _length_):
    以给定的角度围绕指定的原点旋转图像。默认原点是元素的中心。
    设置这些属性时，`Image` 不能有子元素。
-   **`source`** (_in_ _image_): 要加载的图像。使用 `@image-url("...")` 宏指定图像的位置。
-   **`source-clip-x`**、**`source-clip-y`**、**`source-clip-width`** 和 **`source-clip-height`** (_in_ _int_): 在源图像坐标中定义渲染的源图像的区域。默认情况下，整个源图像是可见的：
    | 属性 | 默认绑定 |
    |----------|---------------|
    | `source-clip-x` | `0` |
    | `source-clip-y` | `0` |
    | `source-clip-width` | `source.width - source-clip-x` |
    | `source-clip-height` | `source.height - source-clip-y` |
-   **`width`** 和 **`height`** (_in_ _length_): 图像在屏幕上显示的宽度和高度。默认值是 **`source`** 图像提供的大小。如果 `Image` **不是**在布局中，并且只有**一个**大小被指定，则另一个默认为指定值，根据 **`source`** 图像的纵横比缩放。

### 示例

```slint
export component Example inherits Window {
    width: 100px;
    height: 100px;
    VerticalLayout {
        Image {
            source: @image-url("https://slint.dev/logo/slint-logo-full-light.svg");
            // image-fit default is `contain` when in layout, preserving aspect ratio
        }
        Image {
            source: @image-url("https://slint.dev/logo/slint-logo-full-light.svg");
            colorize: red;
        }
    }
}
```

保持纵横比缩放：

```slint
export component Example inherits Window {
    width: 100px;
    height: 150px;
    VerticalLayout {
        Image {
            source: @image-url("https://slint.dev/logo/slint-logo-full-light.svg");
            width: 100px;
            // implicit default, preserving aspect ratio:
            // height: self.width * natural_height / natural_width;
        }
    }
}
```

## `Path`

`Path` 元素允许渲染一个通用形状，由不同的几何命令组成。路径形状可以填充和轮廓。

当不在布局中时，其宽度或高度默认为未指定时父元素的 100%。

路径可以通过两种不同的方式定义：

-   使用 SVG 路径命令作为字符串
-   使用 `.slint` 标记中的路径命令元素。

几何命令中使用的坐标是路径的想象坐标系中的坐标。
在屏幕上渲染时，形状相对于 `x` 和 `y` 属性绘制。如果 `width` 和 `height` 属性为非零，则整个形状将根据这些边界进行缩放。

### 通用路径属性

-   **`fill`** (_in_ _brush_): 用于填充路径形状的颜色。
-   **`fill-rule`** (_in_ _enum [`FillRule`](enums.md#fillrule)_): 用于路径的填充规则。 (默认值：`nonzero`)
-   **`stroke`** (_in_ _brush_): 用于绘制路径轮廓的颜色。
-   **`stroke-width`** (_in_ _length_): 轮廓的宽度。
-   **`width`** (_in_ _length_): 如果非零，则路径将缩放以适合指定的宽度。
-   **`height`** (_in_ _length_): 如果非零，则路径将缩放以适合指定的高度。
-   **`viewbox-x`**/**`viewbox-y`**/**`viewbox-width`**/**`viewbox-height`** (_in_ _float_) 这四个属性允许在路径坐标中定义路径的视口的位置和大小。

    如果 `viewbox-width` 或 `viewbox-height` 小于或等于零，则忽略视口属性，而是使用所有路径元素的边界矩形来定义视口。

-   **`clip`** (_in_ _bool_): 默认情况下，当路径具有视口定义并且元素在其外部渲染时，它们仍然会被渲染。当此属性设置为 `true` 时，将在视口边界处剪切渲染。此属性必须是文字 `true` 或 `false`（默认值：`false`）

#### 使用 SVG 命令的路径

SVG 是一种流行的文件格式，用于定义可缩放的图形，这些图形通常由路径组成。在 SVG 中，路径由 [命令](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/d#path_commands) 组成，这些命令又写成字符串。在 `.slint` 中，路径命令提供给 `commands` 属性。下面的示例渲染由 `line-to`、`move-to` 和 `arc` 命令组成的弧和矩形形状：

```slint
export component Example inherits Path {
    width: 100px;
    height: 100px;
    commands: "M 0 0 L 0 100 A 1 1 0 0 0 100 100 L 100 0 Z";
    stroke: red;
    stroke-width: 1px;
}
```

命令作为属性提供：

-   **`commands`** (_in_ _string_): 根据 SVG 路径规范提供命令的字符串。

#### 使用 SVG 路径元素的路径

路径的形状也可以使用类似 SVG 路径命令的元素来描述，但使用 `.slint` 标记语法。前面使用 SVG 命令的示例也可以这样写：

```slint
export component Example inherits Path {
    width: 100px;
    height: 100px;
    stroke: blue;
    stroke-width: 1px;

    MoveTo {
        x: 0;
        y: 0;
    }
    LineTo {
        x: 0;
        y: 100;
    }
    ArcTo {
        radius-x: 1;
        radius-y: 1;
        x: 100;
        y: 100;
    }
    LineTo {
        x: 100;
        y: 0;
    }
    Close {
    }
}
```

注意路径元素的坐标不使用单位 - 它们在可缩放路径的想象坐标系中运行。

##### `Path` 的 `MoveTo` 子元素

`MoveTo` 子元素关闭当前子路径（如果存在），并将当前点移动到由 `x` 和 `y` 属性指定的位置。随后的元素（如 `LineTo`）将使用此新位置作为其起点，因此这将开始一个新的子路径。

###### 属性

-   **`x`** (_in_ _float_): 新当前点的 x 位置。
-   **`y`** (_in_ _float_): 新当前点的 y 位置。

##### `Path` 的 `LineTo` 子元素

`LineTo` 子元素描述了从路径的当前位置到由 `x` 和 `y` 属性指定的位置的线。

###### 属性

-   **`x`** (_in_ _float_): 线的目标 x 位置。
-   **`y`** (_in_ _float_): 线的目标 y 位置。

##### `Path` 的 `ArcTo` 子元素

`ArcTo` 子元素描述了从路径的当前位置到由 `x` 和 `y` 属性指定的位置的线。

###### 属性

-   **`large-arc`** (_in_ _bool_): 在封闭椭圆的两个弧中，此标志选择要呈现的较大的弧。如果该属性为 `false`，则呈现较短的弧。
-   **`radius-x`** (_in_ _float_): 椭圆的 x 半径。
-   **`radius-y`** (_in_ _float_): 椭圆的 y 半径。
-   **`sweep`** (_in_ _bool_): 如果该属性为 `true`，则弧将作为顺时针弧绘制；否则为逆时针。
-   **`x-rotation`** (_in_ _float_): 椭圆的 x 轴将以此属性的值旋转，以角度表示，范围为 0 到 360。
-   **`x`** (_in_ _float_): 线的目标 x 位置。
-   **`y`** (_in_ _float_): 线的目标 y 位置。

##### `Path` 的 `CubicTo` 子元素

`CubicTo` 子元素描述了从路径的当前位置到由 `x` 和 `y` 属性指定的位置的平滑贝塞尔曲线，使用由其各自属性指定的两个控制点。

###### 属性

-   **`control-1-x`** (_in_ _float_): 曲线的第一个控制点的 x 坐标。
-   **`control-1-y`** (_in_ _float_): 曲线的第一个控制点的 y 坐标。
-   **`control-2-x`** (_in_ _float_): 曲线的第二个控制点的 x 坐标。
-   **`control-2-y`** (_in_ _float_): 曲线的第二个控制点的 y 坐标。
-   **`x`** (_in_ _float_): 曲线的目标 x 位置。
-   **`y`** (_in_ _float_): 曲线的目标 y 位置。

##### `Path` 的 `QuadraticTo` 子元素

`QuadraticTo` 子元素描述了从路径的当前位置到由 `x` 和 `y` 属性指定的位置的平滑贝塞尔曲线，使用由 `control-x` 和 `control-y` 属性指定的控制点。

###### 属性

-   **`control-x`** (_in_ _float_): 曲线的控制点的 x 坐标。
-   **`control-y`** (_in_ _float_): 曲线的控制点的 y 坐标。
-   **`x`** (_in_ _float_): 曲线的目标 x 位置。
-   **`y`** (_in_ _float_): 曲线的目标 y 位置。

##### `Path` 的 `Close` 子元素

`Close` 元素关闭当前子路径，并绘制一条直线从当前位置到路径的起点。

## `PopupWindow`

使用此元素显示弹出窗口，例如工具提示或弹出菜单。

注意：不允许从 `PopupWindow` 外部访问其内部元素的属性。

### 属性

-   **`close-on-click`** (_in_ _bool_): 默认情况下，当用户单击时，`PopupWindow` 将关闭。将此设置为 false 可以防止该行为，并使用 `close()` 函数手动关闭它。 (默认值：true)

### 函数

-   **`show()`** 显示弹出窗口。
-   **`close()`** 关闭弹出窗口。如果将 `close-on-click` 属性设置为 false，则使用此函数。

### 示例

```slint
export component Example inherits Window {
    width: 100px;
    height: 100px;

    popup := PopupWindow {
        Rectangle { height:100%; width: 100%; background: yellow; }
        x: 20px; y: 20px; height: 50px; width: 50px;
    }

    TouchArea {
        height:100%; width: 100%;
        clicked => { popup.show(); }
    }
}
```

## `Rectangle`

默认情况下，`Rectangle` 是一个什么都不显示的空项。通过设置颜色或配置边框，可以在屏幕上绘制矩形。

当不在布局中时，其宽度或高度默认为未指定时父元素的 100%。

### 属性

-   **`background`** (_in_ _brush_): 此 `Rectangle` 的背景画刷，通常是颜色。 (默认值：`transparent`)
-   **`border-color`** (_in_ _brush_): 边框的颜色。 (默认值：`transparent`)
-   **`border-radius`** (_in_ _length_): 半径的大小。 (默认值：0)
-   **`border-width`** (_in_ _length_): 边框的宽度。 (默认值：0)
-   **`clip`** (_in_ _bool_): 默认情况下，当一个元素比另一个元素大或超出另一个元素时，它仍然会显示。当此属性设置为 `true` 时，此 `Rectangle` 的子元素将被剪切到矩形的边框。 (默认值：`false`)

### 示例

```slint
export component Example inherits Window {
    width: 270px;
    height: 100px;

    Rectangle {
        x: 10px;
        y: 10px;
        width: 50px;
        height: 50px;
        background: blue;
    }

    // Rectangle with a border
    Rectangle {
        x: 70px;
        y: 10px;
        width: 50px;
        height: 50px;
        background: green;
        border-width: 2px;
        border-color: red;
    }

    // Transparent Rectangle with a border and a radius
    Rectangle {
        x: 140px;
        y: 10px;
        width: 50px;
        height: 50px;
        border-width: 4px;
        border-color: black;
        border-radius: 10px;
    }

    // A radius of width/2 makes it a circle
    Rectangle {
        x: 210px;
        y: 10px;
        width: 50px;
        height: 50px;
        background: yellow;
        border-width: 2px;
        border-color: blue;
        border-radius: self.width/2;
    }
}
```

## `TextInput`

`TextInput` 是一个较低级别的项，用于显示文本并允许输入文本。

当不在布局中时，其宽度或高度默认为未指定时父元素的 100%。

### 属性

-   **`color`** (_in_ _brush_): 文本的颜色 (默认值：取决于样式)
-   **`font-family`** (_in_ _string_): 用于呈现文本的字体系列的名称。
-   **`font-size`** (_in_ _length_): 文本的字体大小。
-   **`font-weight`** (_in_ _int_): 字体的粗细。值的范围从 100（最轻）到 900（最厚）。400 是正常的重量。
-   **`font-italic`** (_in_ _bool_): 字体是否应该以斜体显示。 (默认值：false)
-   **`has-focus`** (_out_ _bool_): 当它被聚焦时，`TextInput` 将其设置为 `true`。只有在这种情况下，它才会接收 [`KeyEvent`](structs.md#keyevent)。
-   **`horizontal-alignment`** (_in_ _enum [`TextHorizontalAlignment`](enums.md#texthorizontalalignment)_): 文本的水平对齐方式。
-   **`input-type`** (_in_ _enum [`InputType`](enums.md#inputtype)_): 使用此属性为特殊输入（例如密码字段）配置 `TextInput`。 (默认值：`text`)
-   **`letter-spacing`** (_in_ _length_): 字母间距允许更改字形之间的间距。正值增加间距，负值减少距离。 (默认值：0)
-   **`read-only`** (_in_ _bool_): 当设置为 `true` 时，禁用通过键盘和鼠标编辑文本，但仍然启用选择文本以及以编程方式编辑文本。 (默认值：`false`)
-   **`selection-background-color`** (_in_ _color_): 选择的背景颜色。
-   **`selection-foreground-color`** (_in_ _color_): 选择的前景颜色。
-   **`single-line`** (_in_ _bool_): 当设置为 `true` 时，文本始终呈现为单行，而不管文本中的换行符。 (默认值：`true`)
-   **`text-cursor-width`** (_in_ _length_): 文本光标的宽度。 (默认值：由所选的小部件样式在运行时提供)
-   **`text`** (_in-out_ _string_): 用户呈现和可编辑的文本。
-   **`vertical-alignment`** (_in_ _enum [`TextVerticalAlignment`](enums.md#textverticalalignment)_): 文本的垂直对齐方式。
-   **`wrap`** (_in_ _enum [`TextWrap`](enums.md#textwrap)_): 文本输入的方式。仅当 `single-line` 为 false 时才有意义。 (默认值：no-wrap)

### 函数

-   **`focus()`** 调用此函数以聚焦文本输入，并使其接收未来的键盘事件。
-   **`select-all()`** 选择所有文本。
-   **`clear-selection()`** 清除选择。
-   **`copy()`** 将所选文本复制到剪贴板。
-   **`cut()`** 将所选文本复制到剪贴板，并将其从可编辑区域中删除。
-   **`paste()`** 将剪贴板的文本内容粘贴到光标位置。

### 回调

-   **`accepted()`**: 按下回车键时调用。
-   **`cursor-position-changed(Point)`**: 光标移动到由 [_`Point`_](structs.md#point) 参数描述的新位置 (x, y)。
-   **`edited()`**: 当文本因用户修改而更改时调用。

### 示例

```slint
export component Example inherits Window {
    width: 270px;
    height: 100px;

    TextInput {
        text: "Replace me with a name";
    }
}
```

## `Text`

`Text` 元素负责呈现文本。除了指定要呈现的文本的 `text` 属性之外，它还允许通过 `font-family`、`font-size`、`font-weight` 和 `color` 属性来配置不同的视觉方面。

`Text` 元素可以将长文本分成多行文本。`text` 属性中的换行符（`\n`）将触发手动换行。对于自动换行，需要将 `wrap` 属性设置为除 `no-wrap` 之外的值，并且重要的是为 `Text` 元素指定 `width` 和 `height`，以便知道在哪里换行。建议将 `Text` 元素放在布局中，并根据可用的屏幕空间和文本本身设置 `width` 和 `height`。

### 属性

-   **`color`** (_in_ _brush_): 文本的颜色。 (默认值：取决于样式)
-   **`font-family`** (_in_ _string_): 用于呈现文本的字体系列的名称。
-   **`font-size`** (_in_ _length_): 文本的字体大小。
-   **`font-weight`** (_in_ _int_): 字体的粗细。值的范围从 100（最轻）到 900（最厚）。400 是正常的重量。
-   **`font-italic`** (_in_ _bool_): 字体是否应该以斜体显示。 (默认值：false)
-   **`horizontal-alignment`** (_in_ _enum [`TextHorizontalAlignment`](enums.md#texthorizontalalignment)_): 文本的水平对齐方式。
-   **`letter-spacing`** (_in_ _length_): 字母间距允许更改字形之间的间距。正值增加间距，负值减少距离。 (默认值：0)
-   **`overflow`** (_in_ _enum [`TextOverflow`](enums.md#textoverflow)_): 文本溢出时发生的情况 (默认值：clip)。
-   **`text`** (_in_ _[string](../syntax/types.md#strings)_): 要呈现的文本。
-   **`vertical-alignment`** (_in_ _enum [`TextVerticalAlignment`](enums.md#textverticalalignment)_): 文本的垂直对齐方式。
-   **`wrap`** (_in_ _enum [`TextWrap`](enums.md#textwrap)_): 文本换行的方式 (默认值：`no-wrap`)。

### 示例

这个例子显示红色的文本“Hello World”，使用默认字体：

```slint
export component Example inherits Window {
    width: 270px;
    height: 100px;

    Text {
        x:0;y:0;
        text: "Hello World";
        color: red;
    }
}
```

这个例子将较长的文本段分成多行文本，通过设置 `wrap` 策略并为 `Text` 元素分配有限的 `width` 和足够的 `height`，以便文本向下流动：

```slint
export component Example inherits Window {
    width: 270px;
    height: 300px;

    Text {
        x:0;
        text: "This paragraph breaks into multiple lines of text";
        wrap: word-wrap;
        width: 150px;
        height: 100%;
    }
}
```

## `TouchArea`

使用 `TouchArea` 控制当鼠标触摸或与其交互时覆盖的区域会发生什么。

当不在布局中时，其宽度或高度默认为未指定时父元素的 100%。

### 属性

-   **`has-hover`** (_out_ _bool_): 当鼠标悬停在其上时，`TouchArea` 将其设置为 `true`。
-   **`mouse-cursor`** (_in_ _enum [`MouseCursor`](enums.md#mousecursor)_): 当鼠标悬停在 `TouchArea` 上时的鼠标光标类型。
-   **`mouse-x`**、**`mouse-y`** (_out_ _length_): `TouchArea` 将其设置为鼠标在其中的位置。
-   **`pressed-x`**、**`pressed-y`** (_out_ _length_): `TouchArea` 将其设置为鼠标在最后一次按下的位置。
-   **`pressed`** (_out_ _bool_): 当鼠标在其上按下时，`TouchArea` 将其设置为 `true`。

### 回调

-   **`clicked()`**: 当点击时调用：鼠标在此元素上按下，然后释放。
-   **`double-clicked()**`: 当双击时调用。鼠标在此元素上按下并释放两次。分配此回调将导致 `clicked` 回调被延迟，以便 Slint 可以检测第一次单击是单击还是双击的第一半。
-   **`moved()`**: 鼠标已移动。只有当鼠标也被按下时，才会调用此函数。另请参阅 **pointer-event(PointerEvent)**。
-   **`pointer-event(PointerEvent)`**: 当按下或释放按钮或指针移动时调用。[_`PointerEvent`_](structs.md#pointerevent) 参数包含有关要滚动多少以及在哪个方向上的信息。返回的 [`EventResult`](enums.md#eventresult) 指示接受还是忽略事件。忽略的事件将转发到父元素。
-   **`scroll-event(PointerScrollEvent) -> EventResult`**: 当鼠标滚轮旋转或进行其他滚动手势时调用。[_`PointerScrollEvent`_](structs.md#pointerscrollevent) 参数包含有关要滚动多少以及在哪个方向上的信息。返回的 [`EventResult`](enums.md#eventresult) 指示接受还是忽略事件。忽略的事件将转发到父元素。
-   **`scroll-event(PointerScrollEvent) -> EventResult`**: 当鼠标滚轮旋转或进行其他滚动手势时调用。[_`PointerScrollEvent`_](structs.md#pointerscrollevent) 参数包含有关要滚动多少以及在哪个方向上的信息。返回的 [`EventResult`](enums.md#eventresult) 指示接受还是忽略事件。忽略的事件将转发到父元素。

### 示例

```slint
export component Example inherits Window {
    width: 200px;
    height: 100px;
    area := TouchArea {
        width: parent.width;
        height: parent.height;
        clicked => {
            rect2.background = #ff0;
        }
    }
    Rectangle {
        x:0;
        width: parent.width / 2;
        height: parent.height;
        background: area.pressed ? blue: red;
    }
    rect2 := Rectangle {
        x: parent.width / 2;
        width: parent.width / 2;
        height: parent.height;
    }
}
```

## `VerticalLayout` 和 `HorizontalLayout`

这些布局将其子元素垂直或水平放置在一起。元素的大小可以使用 `width` 或 `height` 属性固定，或者如果它们未设置，则布局将根据最小和最大大小以及拉伸因子来计算元素的大小。

### 属性

-   **`spacing`** (_in_ _length_): 布局中元素之间的距离。
-   **`padding`** (_in_ _length_): 布局内的填充。
-   **`padding-left`**、**`padding-right`**、**`padding-top`** 和 **`padding-bottom`** (_in_ _length_): 设置这些属性以覆盖特定边上的填充。
-   **`alignment`** (_in_ _enum [`LayoutAlignment`](enums.md#layoutalignment)_): 设置对齐方式。与 CSS flex box 匹配。

### 示例

```slint
export component Foo inherits Window {
    width: 200px;
    height: 100px;
    HorizontalLayout {
        spacing: 5px;
        Rectangle { background: red; width: 10px; }
        Rectangle { background: blue; min-width: 10px; }
        Rectangle { background: yellow; horizontal-stretch: 1; }
        Rectangle { background: green; horizontal-stretch: 2; }
    }
}
```

## `Window`

`Window` 是屏幕上可见的元素树的根。

`Window` 的几何形状将受其布局约束的限制：设置 `width` 将导致固定宽度，窗口管理器将尊重 `min-width` 和 `max-width`，因此窗口不能调整大小更大或更小。初始宽度可以使用 `preferred-width` 属性控制。高度也是如此。

### 属性

-   **`always-on-top`** (_in_ _bool_): 是否应将窗口放置在窗口管理器支持的所有其他窗口之上。
-   **`background`** (_in_ _brush_): `Window` 的背景画刷。 (默认值：取决于样式)
-   **`default-font-family`** (_in_ _string_): 用作此窗口中文本元素的默认字体系列的名称，这些元素没有设置其 `font-family` 属性。
-   **`default-font-size`** (_in-out_ _length_): 用作此窗口中文本元素的默认字体大小，这些元素没有设置其 `font-size` 属性。此属性的值也构成相对字体大小的基础。
-   **`default-font-weight`** (_in_ _int_): 用作此窗口中文本元素的默认字体粗细，这些元素没有设置其 `font-weight` 属性。值的范围从 100（最轻）到 900（最厚）。400 是正常的重量。
-   **`icon`** (_in_ _image_): 在标题栏或窗口管理器支持的任务栏中显示的窗口图标。
-   **`no-frame`** (_in_ _bool_): 是否应该无边框/无框架。
-   **`title`** (_in_ _string_): 在标题栏中显示的窗口标题。