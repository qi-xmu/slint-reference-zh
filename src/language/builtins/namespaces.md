<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 内置命名空间

以下命名空间提供对常见常量的访问，例如特殊键或命名颜色。

## `Colors`

使用颜色命名空间按名称选择颜色。例如，您可以使用 `Colors.aquamarine` 或 `Colors.bisque`。
完整的名称列表非常长。您可以在 [CSS 规范](https://www.w3.org/TR/css-color-3/#svg-color) 中找到完整的列表。

这些函数在全局范围内和 `Colors` 命名空间中都可用。

-   **`rgb(int, int, int) -> color`**, **`rgba(int, int, int, float) -> color`**

返回 CSS 中的颜色。与 CSS 不同，这两个函数实际上是可以接受三个或四个参数的别名。

前 3 个参数可以是介于 0 和 255 之间的数字，也可以是带有 `%` 单位的百分比。
如果存在第四个值，则为介于 0 和 1 之间的 alpha 值。

与 CSS 不同，逗号是必需的。

## `Key`

使用 `Key` 命名空间中的常量来处理没有可打印字符的键的按下。检查 [`KeyEvent`](structs.md#keyevent) 的 `text` 属性的值与下面的常量。

-   **`Backspace`**
-   **`Tab`**
-   **`Return`**
-   **`Escape`**
-   **`Backtab`**
-   **`Delete`**
-   **`Shift`**
-   **`Control`**
-   **`Alt`**
-   **`AltGr`**
-   **`CapsLock`**
-   **`ShiftR`**
-   **`ControlR`**
-   **`Meta`**
-   **`MetaR`**
-   **`UpArrow`**
-   **`DownArrow`**
-   **`LeftArrow`**
-   **`RightArrow`**
-   **`F1`**
-   **`F2`**
-   **`F3`**
-   **`F4`**
-   **`F5`**
-   **`F6`**
-   **`F7`**
-   **`F8`**
-   **`F9`**
-   **`F10`**
-   **`F11`**
-   **`F12`**
-   **`F13`**
-   **`F14`**
-   **`F15`**
-   **`F16`**
-   **`F17`**
-   **`F18`**
-   **`F19`**
-   **`F20`**
-   **`F21`**
-   **`F22`**
-   **`F23`**
-   **`F24`**
-   **`Insert`**
-   **`Home`**
-   **`End`**
-   **`PageUp`**
-   **`PageDown`**
-   **`ScrollLock`**
-   **`Pause`**
-   **`SysReq`**
-   **`Stop`**
-   **`Menu`**

## `Math`

使用 `Math` 命名空间中的函数来执行数学运算。这些函数在全局范围内和 `Math` 命名空间中都可用。

### `abs(float) -> float`

返回绝对值。

### `acos(float) -> angle`、`asin(float) -> angle`、`atan(float) -> angle`、`cos(angle) -> float`、`sin(angle) -> float`、`tan(angle) -> float`

三角函数。注意，应该使用 `deg` 或 `rad` 单位对其进行类型化（例如 `cos(90deg)` 或 `sin(slider.value * 1deg)`）。

### `ceil(float) -> int` 和 `floor(float) -> int`

返回上限或下限。

### `clamp(T, T, T) -> T`

获取 `value`、`minimum` 和 `maximum`，并在 `value > maximum` 时返回 `maximum`，在 `value < minimum` 时返回 `minimum`，在所有其他情况下返回 `value`。

### `log(float, float) -> float`

返回以第二个值为底的第一个值的对数。

### `max(T, T) -> T` 和 `min(T, T) -> T`

返回具有最小（或最大）值的参数。所有参数必须是相同的数字类型。

### `mod(T, T) -> T`

执行模运算，其中 T 是某种数字类型。

### `round(float) -> int`

返回最接近的整数值。

### `sqrt(float) -> float`

平方根。

### `pow(float, float) -> float`

返回第一个值的第二个值的幂。

