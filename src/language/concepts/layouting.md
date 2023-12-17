<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 定位和元素布局

所有的视觉元素都显示在一个窗口中。`x` 和 `y` 属性存储了元素相对于其父元素的坐标。
Slint 通过将父元素的位置添加到元素的位置来确定元素的绝对位置。
如果父元素本身有父元素，那么也会添加到父元素中。这个计算会一直持续到达到顶层元素。

`width` 和 `height` 属性存储了视觉元素的大小。

您可以通过两种方式将元素放置在一个完整的图形用户界面中：

-   显式 - 通过设置 `x`，`y`，`width` 和 `height` 属性。
-   自动 - 使用布局元素。

显式放置适用于具有少量元素的静态场景。布局适用于复杂的用户界面，并帮助创建可扩展的用户界面。
布局元素表达了元素之间的几何关系。

## 显式放置

下面的示例将两个矩形放置在一个窗口中，一个蓝色的和一个绿色的。绿色的矩形是蓝色的子元素：

```slint
// 显式定位
export component Example inherits Window {
    width: 200px;
    height: 200px;
    Rectangle {
        x: 100px;
        y: 70px;
        width: parent.width - self.x;
        height: parent.height - self.y;
        background: blue;
        Rectangle {
            x: 10px;
            y: 5px;
            width: 50px;
            height: 30px;
            background: green;
        }
    }
}
```

两个矩形的位置和内部绿色矩形的大小是固定的。
外部蓝色矩形的大小是使用绑定表达式自动计算的 `width` 和 `height` 属性。
计算结果是底部左侧的角与窗口的角对齐 —— 它在窗口的 `width` 和 `height` 改变时更新。

当指定几何属性的任何一个时，Slint 要求您将单位附加到数字。您可以在两个不同的单位之间进行选择：

-   逻辑像素，使用 `px` 单位后缀。这是推荐的单位。
-   物理像素，使用 `phx` 单位后缀

逻辑像素会自动缩放，以适应系统配置的设备像素比率。
例如，在现代的高 DPI 显示器上，设备像素比率可以是 2，因此每个逻辑像素占用 2 个物理像素。
在旧屏幕上，用户界面会自动缩放而不需要任何适应。

此外，`width` 和 `height` 属性也可以指定为 `%` 百分比单位，该单位相对于父元素应用。
例如，`width: 50%` 表示父元素的一半。

`x` 和 `y` 属性的默认值是使元素在其父元素内居中。

`width` 和 `height` 的默认值取决于元素的类型。
一些元素根据其内容自动调整大小，例如 `Image`，`Text` 和大多数小部件。

以下元素没有内容，当它们没有子元素时，默认填充其父元素：

-   `Rectangle`
-   `TouchArea`
-   `FocusScope`
-   `Flickable`

布局元素也默认填充父元素，而不管它们自己的首选大小。

其他元素（包括没有基类的自定义元素）默认使用其首选大小。

### 首选大小

元素的首选大小可以使用 `preferred-width` 和 `preferred-height` 属性指定。

当没有显式设置时，首选大小取决于子元素，并且是具有更大首选大小的子元素的首选大小，其 `x` 和 `y` 属性未设置。
因此，首选大小是从子元素到父元素计算的，就像其他约束（最大和最小大小）一样，除非显式覆盖。

一个特殊的情况是将首选大小设置为父元素的大小，使用 `100%` 作为值。
例如，这个组件将默认使用父元素的大小：

```slint
component MyComponent {
    preferred-width: 100%;
    preferred-height: 100%;
    // ...
}
```

## 使用布局自动放置

Slint 附带了不同的布局元素，它们自动计算其子元素的位置和大小：

-   `VerticalLayout` / `HorizontalLayout`：子元素沿垂直或水平轴放置。
-   `GridLayout`：子元素在列和行的网格中放置。

您还可以嵌套布局以创建复杂的用户界面。

你可以使用不同的约束条件来调整自动布局，以适应你的用户界面设计。每个元素都有一个最小尺寸、一个最大尺寸和一个首选尺寸。通过以下属性明确设置这些尺寸：

-   `min-width`
-   `min-height`
-   `max-width`
-   `max-height`
-   `preferred-width`
-   `preferred-height`

在布局中，具有指定 `width` 和 `height` 的任何元素都具有固定的大小。

在布局中有额外的空间时，元素可以沿着布局轴拉伸。您可以使用以下属性在元素和其兄弟元素之间控制此拉伸因子：

-   `horizontal-stretch`
-   `vertical-stretch`

值为 `0` 表示元素不会拉伸。如果它们都有一个拉伸因子为 `0`，则所有元素都会均匀拉伸。

这些约束属性的默认值可能取决于元素的内容。如果元素的 `x` 或 `y` 没有设置，这些约束也会自动应用到父元素。

## 布局元素上的常见属性

所有布局元素都具有以下共同属性：

-   `spacing`：这控制子元素之间的间距。
-   `padding`：这指定了布局内的填充，元素和布局边框之间的空间。

为了更细粒度的控制，`padding` 属性可以分割为布局的每一边的属性：

-   `padding-left`
-   `padding-right`
-   `padding-top`
-   `padding-bottom`

## `VerticalLayout` 和 `HorizontalLayout`

`VerticalLayout` 和 `HorizontalLayout` 元素将其子元素放置在列或行中。默认情况下，它们会拉伸或收缩以占用整个空间。您可以根据需要调整元素的对齐方式。

下面的示例将蓝色和黄色矩形放置在一行中，并均匀地拉伸到 `width` 的 200 个逻辑像素：

```slint
// 默认拉伸
export component Example inherits Window {
    width: 200px;
    height: 200px;
    HorizontalLayout {
        Rectangle { background: blue; min-width: 20px; }
        Rectangle { background: yellow; min-width: 30px; }
    }
}
```

在下面的示例中，另一方面，指定矩形应与布局的起始位置（视觉上的左侧）对齐。
这样就不会拉伸矩形，而是保留其指定的最小宽度：

```slint
// 除非指定了对齐方式
export component Example inherits Window {
    width: 200px;
    height: 200px;
    HorizontalLayout {
        alignment: start;
        Rectangle { background: blue; min-width: 20px; }
        Rectangle { background: yellow; min-width: 30px; }
    }
}
```

下面的示例嵌套了两个布局以获得更复杂的场景：

```slint
export component Example inherits Window {
    width: 200px;
    height: 200px;
    HorizontalLayout {
        // 侧面板
        Rectangle { background: green; width: 10px; }

        VerticalLayout {
            padding: 0px;
            // 工具栏
            Rectangle { background: blue; height: 7px; }

            Rectangle {
                border-color: red; border-width: 2px;
                HorizontalLayout {
                    Rectangle { border-color: blue; border-width: 2px; }
                    Rectangle { border-color: green; border-width: 2px; }
                }
            }
            Rectangle {
                border-color: orange; border-width: 2px;
                HorizontalLayout {
                    Rectangle { border-color: black; border-width: 2px; }
                    Rectangle { border-color: pink; border-width: 2px; }
                }
            }
        }
    }
}
```

### 对齐

每个元素的大小是根据其 `width` 或 `height` 来设定的，如果这些属性被指定了的话。如果没有指定，那么它的大小会被设置为最小尺寸，这个最小尺寸是通过 `min-width` 或 `min-height` 属性来设定的，或者是通过内部布局的最小尺寸来设定的，取两者中较大的那个值作为元素的最终大小。

元素根据对齐方式放置。元素的大小只有在布局的 `alignment` 属性为 `LayoutAlignment.stretch`（默认值）时才会大于最小尺寸。

这个例子展示了不同的对齐方式：

```slint
export component Example inherits Window {
    width: 300px;
    height: 200px;
    VerticalLayout {
        HorizontalLayout {
            alignment: stretch;
            Text { text: "stretch (default)"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: start;
            Text { text: "start"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: end;
            Text { text: "end"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: start;
            Text { text: "start"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: center;
            Text { text: "center"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: space-between;
            Text { text: "space-between"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
        HorizontalLayout {
            alignment: space-around;
            Text { text: "space-around"; }
            Rectangle { background: blue; min-width: 20px; }
            Rectangle { background: yellow; min-width: 30px; }
        }
    }
}
```

### 拉伸算法

当 `alignment` 设置为 `stretch`（默认值）时，元素被调整为其最小尺寸，然后额外的空间按照其拉伸因子进行比例分配，这个拉伸因子是通过 `horizontal-stretch` 和 `vertical-stretch` 属性来设定的。拉伸后的大小不会超过最大尺寸。拉伸因子是一个浮点数。那些默认内容大小的元素通常默认为 0，而那些默认为其父元素大小的元素通常默认为 1。
拉伸因子为 0 的元素将保持其最小尺寸，除非所有其他元素的拉伸因子也为 0 或达到其最大尺寸。

例子：

```slint
export component Example inherits Window {
    width: 300px;
    height: 200px;
    VerticalLayout {
        // Same stretch factor (1 by default): the size is divided equally
        HorizontalLayout {
            Rectangle { background: blue; }
            Rectangle { background: yellow;}
            Rectangle { background: green;}
        }
        // Elements with a bigger min-width are given a bigger size before they expand
        HorizontalLayout {
            Rectangle { background: cyan; min-width: 100px;}
            Rectangle { background: magenta; min-width: 50px;}
            Rectangle { background: gold;}
        }
        // Stretch factor twice as big:  grows twice as much
        HorizontalLayout {
            Rectangle { background: navy; horizontal-stretch: 2;}
            Rectangle { background: gray; }
        }
        // All elements not having a maximum width have a stretch factor of 0 so they grow
        HorizontalLayout {
            Rectangle { background: red; max-width: 20px; }
            Rectangle { background: orange; horizontal-stretch: 0; }
            Rectangle { background: pink; horizontal-stretch: 0; }
        }
    }
}
```

### `for`

`VerticalLayout` 和 `HorizontalLayout` 也可以包含 `for` 或 `if` 表达式：

```slint
export component Example inherits Window {
    width: 200px;
    height: 50px;
    HorizontalLayout {
        Rectangle { background: green; }
        for t in [ "Hello", "World", "!" ] : Text {
            text: t;
        }
        Rectangle { background: blue; }
    }
}
```

## 网格布局

网格布局将元素放置在网格中。
每个元素都获得属性 `row`，`col`，`rowspan` 和 `colspan`。
可以使用 `Row` 子元素，也可以显式设置 `row` 属性。
这些属性必须在编译时静态知道，因此不可能使用算术或依赖于属性。截至目前，不允许在网格布局中使用 `for` 或 `if`。

这个例子使用 `Row` 元素

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

这个例子使用 `row` 和 `col` 属性

```slint
export component Foo inherits Window {
    width: 200px;
    height: 150px;
    GridLayout {
        spacing: 0px;
        Rectangle { background: red; }
        Rectangle { background: blue; }
        Rectangle { background: yellow; row: 1; }
        Rectangle { background: green; }
        Rectangle { background: black; col: 2; row: 0; }
    }
}
```
