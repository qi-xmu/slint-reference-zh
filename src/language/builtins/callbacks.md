<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 内置回调

## `init()`

每个元素都隐式声明了一个 `init` 回调函数。您可以将代码块分配给它，当元素被实例化并且所有属性都使用其最终绑定的值初始化后，将调用该代码块。调用的顺序是从内到外的。下面的示例将打印“first”，然后是“second”，然后是“third”：

```slint,no-preview
component MyButton inherits Rectangle {
    in-out property <string> text: "Initial";
    init => {
        // 如果在此处查询 `text`，它将具有值“Hello”。
        debug("first");
    }
}

component MyCheckBox inherits Rectangle {
    init => { debug("second"); }
}

export component MyWindow inherits Window {
    MyButton {
        text: "Hello";
        init => { debug("third"); }
    }
    MyCheckBox {
    }
}
```

不要使用此回调函数来初始化属性，因为这违反了声明性原则。
除非需要，否则避免使用此回调函数，例如，为了通知某些本机代码：

```slint,no-preview
global SystemService  {
    // This callback can be implemented in native code using the Slint API
    callback ensure_service_running();
}

export component MySystemButton inherits Rectangle {
    init => {
        SystemService.ensure_service_running();
    }
    // ...
}
```
