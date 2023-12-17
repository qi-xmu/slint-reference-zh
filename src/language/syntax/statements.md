<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 语句

回调处理程序可以包含复杂的语句：

赋值：

```slint,ignore
clicked => { some-property = 42; }
```

使用 `+=` `-=` `*=` `/=` 进行自赋值

```slint,ignore
clicked => { some-property += 42; }
```

调用回调

```slint,ignore
clicked => { root.some-callback(); }
```

条件语句

```slint,ignore
clicked => {
    if (condition) {
        foo = 42;
    } else if (other-condition) {
        bar = 28;
    } else {
        foo = 4;
    }
}
```

空表达式

```slint,ignore
clicked => { }
// or
clicked => { ; }
```
