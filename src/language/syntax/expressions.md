<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 表达式

表达式是在用户界面中声明关系和连接的强大方式。它们通常用于将基本算术与其他元素的属性访问相结合。当这些属性更改时，表达式会自动重新计算并将新值分配给与表达式关联的属性：

```slint,no-preview
export component Example {
    // declare a property of type int
    in-out property<int> my-property;

    // This accesses the property
    width: root.my-property * 20px;

}
```

当 `my-property` 更改时，宽度也会自动更改。

数字表达式中的算术运算与大多数编程语言中的运算符 `*`、`+`、`-`、`/` 相同：

```slint,no-preview
export component Example {
    in-out property <int> p: 1 * 2 + 3 * 4; // same as (1 * 2) + (3 * 4)
}
```

使用 `+` 连接字符串。

运算符 `&&` 和 `||` 表示布尔值之间的逻辑 _and_ 和 _or_。运算符 `==`、`!=`、`>`、`<`、`>=` 和 `<=` 比较相同类型的值。

通过使用其名称，后跟 `.` 和属性名称，访问元素的属性：

```slint,no-preview
export component Example {
    foo := Rectangle {
        x: 42px;
    }
    x: foo.x;
}
```

三元运算符 `... ? ... : ...` 也是支持的，就像 C 或 JavaScript 中一样：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    Rectangle {
        touch := TouchArea {}
        background: touch.pressed ? #111 : #eee;
        border-width: 5px;
        border-color: !touch.enabled ? #888
            : touch.pressed ? #aaa
            : #555;
    }
}
```
