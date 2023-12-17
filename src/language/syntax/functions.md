<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 函数

在 Slint 中，使用 `function` 关键字声明函数。

每个函数都可以有参数，这些参数在括号内声明，格式为 `name : type`。
这些参数可以在函数体内通过其名称引用。

函数还可以返回值。返回类型在函数签名中的 `->` 后指定。
`return` 关键字在函数体内用于返回声明类型的表达式。
如果函数没有显式返回值，则默认情况下返回最后一个语句的值。

函数可以用 `pure` 关键字注释。
这表示该函数不会引起任何副作用。
有关更多详细信息，请参见 [纯洁性](../concepts/purity.md) 章节。

默认情况下，函数是私有的，不能从外部组件访问。
但是，可以使用 `public` 或 `protected` 关键字修改其可访问性。

- 使用 `public` 注释的函数可以被任何组件访问。
- 使用 `protected` 注释的函数只能被直接继承它的组件访问。

## 示例

```slint,no-preview
export component Example {
    in-out property <int> min;
    in-out property <int> max;
    protected function set-bounds(min: int, max: int) {
        root.min = min;
        root.max = max
    }
    public pure function inbound(x: int) -> int {
        return Math.min(root.max, Math.max(root.min, x));
    }
}
```

在上面的示例中，`set-bounds` 是一个受保护的函数，用于更新根组件的 `min` 和 `max` 属性。
`inbound` 函数是一个公共的、纯粹的函数，它接受一个整数 `x`，并返回在 `min` 和 `max` 边界内的值。
