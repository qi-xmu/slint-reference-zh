<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 动画

使用 `animate` 关键字为属性声明动画，如下所示：

```slint
export component Example inherits Window {
    preferred-width: 100px;
    preferred-height: 100px;

    background: area.pressed ? blue : red;
    animate background {
        duration: 250ms;
    }

    area := TouchArea {}
}
```

这将在颜色属性更改时每 100ms 动画一次。

使用以下参数微调动画：

-   `delay`：开始动画之前等待的时间量
-   `duration`：动画完成所需的时间量
-   `iteration-count`：动画应运行的次数。负值指定无限次重播。可能的分数值。
    有关永久运行的动画，请参见 [`animation-tick()`](../builtins/functions.md#animation-tick-duration)。
-   `easing`：可以是以下任何内容。有关可视参考，请参见 [`easings.net`](https://easings.net/)：

    -   `linear`
    -   `ease-in-quad`
    -   `ease-out-quad`
    -   `ease-in-out-quad`
    -   `ease`
    -   `ease-in`
    -   `ease-out`
    -   `ease-in-out`
    -   `ease-in-quart`
    -   `ease-out-quart`
    -   `ease-in-out-quart`
    -   `ease-in-quint`
    -   `ease-out-quint`
    -   `ease-in-out-quint`
    -   `ease-in-expo`
    -   `ease-out-expo`
    -   `ease-in-out-expo`
    -   `ease-in-sine`
    -   `ease-out-sine`
    -   `ease-in-out-sine`
    -   `ease-in-back`
    -   `ease-out-back`
    -   `ease-in-out-back`
    -   `ease-in-circ`
    -   `ease-out-circ`
    -   `ease-in-out-circ`
    -   `ease-in-elastic`
    -   `ease-out-elastic`
    -   `ease-in-out-elastic`
    -   `ease-in-bounce`
    -   `ease-out-bounce`
    -   `ease-in-out-bounce`
    -   `cubic-bezier(a, b, c, d)` 如 CSS 中所示

    缓动示例也可以在 `gallery` 示例的 `Easings` 选项卡中找到。


也可以使用相同的动画来动画化多个属性，因此：

```slint,ignore
animate x, y { duration: 100ms; easing: ease-out-bounce; }
```

与

```slint,ignore
animate x { duration: 100ms; easing: ease-out-bounce; }
animate y { duration: 100ms; easing: ease-out-bounce; }
```

是一样的。
