<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 状态

`states` 语句允许一次声明多个元素的状态和属性：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;
    default-font-size: 24px;

    label := Text { }
    ta := TouchArea {
        clicked => {
            active = !active;
        }
    }
    property <bool> active: true;
    states [
        active when active && !ta.has-hover: {
            label.text: "Active";
            root.background: blue;
        }
        active-hover when active && ta.has-hover: {
            label.text: "Active\nHover";
            root.background: green;
        }
        inactive when !active: {
            label.text: "Inactive";
            root.background: gray;
        }
    ]
}
```

在此示例中，根据 `active` 布尔属性的值和 `TouchArea` 的 `has-hover`，定义了 `active` 和 `active-hover` 状态。当用户用鼠标悬停在示例上时，它将在蓝色和绿色背景之间切换，并相应地调整文本标签。单击切换 `active` 属性，从而进入 `inactive` 状态。

## 过渡

过渡将动画绑定到状态更改。

此示例定义了两个过渡。首先使用 `out` 关键字在离开 `disabled` 状态时为所有属性动画 800ms。第二个过渡使用 `in` 关键字在进入 `down` 状态时动画背景。

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    text := Text { text: "hello"; }
    in-out property<bool> pressed;
    in-out property<bool> is-enabled;

    states [
        disabled when !root.is-enabled : {
            background: gray; // same as root.background: gray;
            text.color: white;
            out {
                animate * { duration: 800ms; }
            }
        }
        down when pressed : {
            background: blue;
            in {
                animate background { duration: 300ms; }
            }
        }
    ]
}
```
