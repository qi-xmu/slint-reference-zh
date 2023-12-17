<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 类型

Slint 中的所有属性都有一个类型。Slint 知道以下基本类型：

| 类型                 | 描述                                                                                                                                                                                                                                                                                                                                      | 默认值 |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| `angle`              | 角度测量，对应于文字 `90deg`、`1.2rad`、`0.25turn`                                                                                                                                                                                                                                                                                       | 0deg   |
| `bool`               | 布尔值，其值可以是 `true` 或 `false`。                                                                                                                                                                                                                                                                                                    | false  |
| `brush`              | 笔刷是一种特殊类型，可以从颜色或渐变规范初始化。有关更多信息，请参见[颜色和笔刷部分](#id3)。                                                                                                                                                                                                                               | transparent |
| `color`              | RGB 颜色，带有 alpha 通道，每个通道的精度为 8 位。支持 CSS 颜色名称以及十六进制颜色编码，例如 `#RRGGBBAA` 或 `#RGB`。                                                                                                                                                                                                                       | transparent |
| `duration`           | 动画的持续时间类型。后缀如 `ms`（毫秒）或 `s`（秒）用于指示精度。                                                                                                                                                                                                                                                                         | 0ms    |
| `easing`             | 属性动画允许指定缓动曲线。有效值为 `linear`（值线性插值）和 CSS 中已知的[四个常见的立方贝塞尔函数](https://developer.mozilla.org/en-US/docs/Web/CSS/easing-function#Keywords_for_common_cubic-bezier_easing_functions)：`ease`、`ease_in`、`ease_in_out`、`ease_out`。 | linear |
| `float`              | 有符号的 32 位浮点数。带有 `%` 后缀的数字会自动除以 100，因此例如 `30%` 等同于 `0.30`。                                                                                                                                                                                                                                                    | 0      |
| `image`              | 对图像的引用，可以使用 `@image-url("...")` 构造进行初始化                                                                                                                                                                                                                                                                                | empty image |
| `int`                | 有符号整数。                                                                                                                                                                                                                                                                                                                              | 0      |
| `length`             | 用于 `x`、`y`、`width` 和 `height` 坐标的类型。对应于文字 `1px`、`1pt`、`1in`、`1mm` 或 `1cm`。它可以在绑定运行的上下文中转换为长度，其中有一个访问设备像素比的访问。                                                                                                                                                    | 0px    |
| `percent`            | 有符号的 32 位浮点数，被解释为百分比。分配给此类型属性的文字字面值必须具有 `%` 后缀。                                                                                                                                                                                                                                                    | 0%     |
| `physical-length`    | 这是一定数量的物理像素。要将整数转换为长度单位，可以简单地乘以 `1px`。或者要将长度转换为浮点数，可以除以 `1phx`。                                                                                                                                                                                                                           | 0phx   |
| `relative-font-size` | 相对字体大小因子，与 `Window.default-font-size` 相乘，可以转换为 `length`。                                                                                                                                                                                                                                                              | 0rem   |
| `string`             | UTF-8 编码的引用计数字符串。                                                                                                                                                                                                                                                                                                              | `""`   |

请参阅特定于语言的 API 参考，了解这些类型如何映射到不同编程语言的 API。

## 字符串

任何由引号括起来的 utf-8 编码字符序列都是 `string`：`"foo"`。

可以将转义序列嵌入字符串中，以插入否则很难插入的字符：

| 转义            | 结果                                                                                          |
| --------------- | ----------------------------------------------------------------------------------------------- |
| `\"`            | `"`                                                                                             |
| `\\`            | `\`                                                                                             |
| `\n`            | 换行                                                                                          |
| `\u{x}`         | 其中 `x` 是十六进制数，扩展为由该数字表示的 Unicode 代码点                                     |
| `\{expression}` | 表达式的结果                                                                                  |

在未转义的 `\` 之后的任何其他内容都是错误的。

```slint,no-preview
export component Example inherits Text {
    text: "hello";
}
```

注意：`\{...}` 语法在 Rust 中的 `slint!` 宏中无效。

## 颜色和笔刷

颜色文字遵循 CSS 的语法：

```slint,no-preview
export component Example inherits Window {
    background: blue;
    property<color> c1: #ffaaff;
    property<brush> b2: Colors.red;
}
```

除了纯色之外，许多元素的属性都是 `brush` 类型而不是 `color` 类型。
笔刷是一种类型，可以是颜色或渐变。
然后，笔刷用于填充元素或绘制轮廓。

CSS 颜色名称仅在类型为 `color` 或 `brush` 的表达式中有效。否则，您可以访问 `Colors` 命名空间中的颜色。

### 方法

所有颜色和笔刷都定义以下方法：

-   **`brighter(factor: float) -> brush`**

    返回一个新颜色，该颜色的亮度增加了指定的因子。
    例如，如果因子为 0.5（或例如 50%），则返回的颜色会变亮 50%。负因子
    降低亮度。

-   **`darker(factor: float) -> brush`**

    返回一个新颜色，该颜色的亮度降低了指定的因子。
    例如，如果因子为 .5（或例如 50%），则返回的颜色会变暗 50%。负因子
    增加亮度。

-   **`mix(other: brush, factor: float) -> brush`**

    返回一个新颜色，该颜色是该颜色和 `other` 的混合，比例
    因子由 \a factor（将被夹紧为 `0.0` 和 `1.0` 之间）。

-  **`transparentize(factor: float) -> brush`**

    返回一个新颜色，其不透明度降低了 `factor`。
    透明度通过将 alpha 通道乘以 `(1 - factor)` 获得。

-  **`with_alpha(alpha: float) -> brush`**

    返回一个新颜色，其 alpha 值设置为 `alpha`（介于 0 和 1 之间）

### 线性渐变

线性渐变描述了平滑的、多彩的表面。
它们使用角度和一系列颜色停止来指定。
颜色将在指定角度旋转的想象线的轴上线性插值。
这称为线性渐变，使用以下签名的 `@linear-gradient` 宏指定：

**`@linear-gradient(angle, color percentage, color percentage, ...)`**

宏的第一个参数是一个角度（见[类型](types.md)）。
渐变线的起点将被旋转指定的值。

接下来是一个或多个颜色停止，描述为 `color` 值和 `percentage` 的空格分隔对。
颜色指定线性颜色插值在渐变轴上的指定百分比处达到的值。

以下示例显示了一个矩形，它用线性渐变填充，该渐变从浅蓝色开始插值到中心的非常浅的色调，然后以橙色色调结束：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    Rectangle {
        background: @linear-gradient(90deg, #3f87a6 0%, #ebf8e1 50%, #f69d3c 100%);
    }
}
```

### 径向渐变

线性渐变就像真正的渐变，但是颜色在圆圈中插值，而不是沿着线插值。
要描述一个读取渐变，请使用 `@radial-gradient` 宏，其签名如下：

**`@radial-gradient(circle, color percentage, color percentage, ...)`**

宏的第一个参数始终是 `circle`，因为只支持圆形渐变。
语法基于 CSS `radial-gradient` 函数。

示例：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;
    Rectangle {
        background: @radial-gradient(circle, #f00 0%, #0f0 50%, #00f 100%);
    }
}
```

## 图像

`image` 类型是对图像的引用。它使用 `@image-url("...")` 构造定义。
`@image-url` 函数中的地址必须在编译时已知。

Slint 在以下位置查找图像：

1. 绝对路径或相对于当前 `.slint` 文件的路径。
2. 编译器用于查找 `.slint` 文件的包含路径。

使用其 `width` 和 `height` 属性访问 `image` 的尺寸。

```slint
export component Example inherits Window {
    preferred-width: 150px;
    preferred-height: 50px;

    in property <image> some_image: @image-url("https://slint.dev/logo/slint-logo-full-light.svg");

    Text {
        text: "The image is " + some_image.width + "x" + some_image.height;
    }
}
```

## 结构体

使用 `struct` 关键字定义命名结构：

```slint,no-preview
export struct Player  {
    name: string,
    score: int,
}

export component Example {
    in-out property <Player> player: { name: "Foo", score: 100 };
}
```

结构体的默认值是初始化所有字段并将其设置为其默认值。

### 匿名结构体

使用 `{ identifier1: type2, identifier1: type2 }` 语法声明匿名结构体，并使用
`{ identifier1: expression1, identifier2: expression2  }` 初始化它们。

最后一个表达式或类型后面可以有一个逗号。

```slint,no-preview
export component Example {
    in-out property <{name: string, score: int}> player: { name: "Foo", score: 100 };
    in-out property <{a: int, }> foo: { a: 3 };
}
```

## 枚举

使用 `enum` 关键字定义枚举：

```slint,no-preview
export enum CardSuit { clubs, diamonds, hearts, spade }

export component Example {
    in-out property <CardSuit> card: spade;
    out property <bool> is-clubs: card == CardSuit.clubs;
}
```

可以通过使用枚举的名称和值的名称之间的点来引用枚举值。 （例如：`CardSuit.spade`）

可以在枚举类型的绑定中省略枚举的名称，或者如果回调的返回值是该枚举，则可以省略枚举的名称。

每个枚举类型的默认值始终是第一个值。

## 数组和模型

通过在数组元素的类型周围包装 `[` 和 `]` 方括号来声明数组。

数组文字以及保存数组的属性在 `for` 表达式中充当模型。

```slint,no-preview
export component Example {
    in-out property <[int]> list-of-int: [1,2,3];
    in-out property <[{a: int, b: string}]> list-of-structs: [{ a: 1, b: "hello" }, {a: 2, b: "world"}];
}
```

数组定义以下操作：

-   **`array.length`**：可以查询数组和模型的长度，使用内置的 `.length` 属性。
-   **`array[index]`**：索引运算符检索数组的单个元素。

超出数组的访问将返回默认构造的值。

```slint,no-preview
export component Example {
    in-out property <[int]> list-of-int: [1,2,3];

    out property <int> list-len: list-of-int.length;
    out property <int> first-int: list-of-int[0];
}
```

## 转换

Slint 支持不同类型之间的转换。为了使 UI 描述更加健壮，需要显式的转换，但是允许在某些类型之间进行隐式转换以方便使用。

可以进行以下转换：

-   `int` 可以隐式转换为 `float`，反之亦然。
-   `int` 和 `float` 可以隐式转换为 `string`。
-   `physical-length` 和 `length` 可以在已知像素比的上下文中相互转换。
-   单位类型（`length`、`physical-length`、`duration` 等）不能转换为数字（`float` 或 `int`），但是可以将其除以自身以得到数字。类似地，数字可以乘以其中一个单位。所以可以通过乘以 `1px` 或除以 `1px` 来进行类型转换。
-   结构类型与另一个结构类型转换，如果它们具有相同的属性名称并且它们的类型可以转换。源结构可以具有缺少的属性或额外的属性。但不能两者都有。
-   数组通常不能相互转换。如果元素类型是可转换的，则可以转换数组文字。
-   字符串可以通过使用 `to-float` 函数转换为浮点数。如果字符串不是有效数字，则该函数返回 0。您可以使用 `is-float()` 检查字符串是否包含有效数字。

```slint,no-preview
export component Example {
    // OK: int converts to string
    property <{a: string, b: int}> prop1: {a: 12, b: 12 };
    // OK: even if a is missing, it will just have the default value ("")
    property <{a: string, b: int}> prop2: { b: 12 };
    // OK: even if c is too many, it will be discarded
    property <{a: string, b: int}> prop3: { a: "x", b: 12, c: 42 };
    // ERROR: b is missing and c is extra, this doesn't compile, because it could be a typo.
    // property <{a: string, b: int}> prop4: { a: "x", c: 42 };

    property <string> xxx: "42.1";
    property <float> xxx1: xxx.to-float(); // 42.1
    property <bool> xxx2: xxx.is-float(); // true
}
```
