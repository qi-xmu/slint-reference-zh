<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 经典语法

为了保持与早期版本的 Slint 的兼容性，仍然支持使用 `:=` 声明组件和命名结构的pre-1.0语法：

```slint,no-preview
export MyApp := Window {
    //...
}
```

此语法更改还影响属性查找规则和默认元素放置。

在使用新语法定义的组件中，只有在组件内部声明的属性才在作用域内。
默认情况下，父元素将其子元素居中渲染，并将应用所有布局约束。

在使用旧语法定义的组件中，`self` 和 `root` 的基础属性以及组件本身内部定义的所有属性都在作用域内。
元素的位置为 `x: 0` 和 `y: 0`，并且不应用其约束。