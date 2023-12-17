<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 循环

使用 `for`-`in` 语法多次创建元素。

语法如下：`for name[index] in model : id := Element { ... }`

`model` 可以是以下类型之一：

-   整数，此时元素将重复该次数
-   本地声明的 [数组或模型](types.md#id11)，此时元素将为数组或模型中的每个元素实例化。

`name` 将在元素内部可用于查找，就像一个伪属性，其值将设置为模型的值。`index` 是可选的，将设置为模型中此元素的索引。`id` 也是可选的。

## 示例

```slint
export component Example inherits Window {
    preferred-width: 300px;
    preferred-height: 100px;
    for my-color[index] in [ #e11, #1a2, #23d ]: Rectangle {
        height: 100px;
        width: 60px;
        x: self.width * index;
        background: my-color;
    }
}
```

```slint
export component Example inherits Window {
    preferred-width: 50px;
    preferred-height: 50px;
    in property <[{foo: string, col: color}]> model: [
        {foo: "abc", col: #f00 },
        {foo: "def", col: #00f },
    ];
    VerticalLayout {
        for data in root.model: my-repeated-text := Text {
            color: data.col;
            text: data.foo;
        }
    }
}
```
