<!-- Copyright © SixtyFPS GmbH <info@slint.dev> ; SPDX-License-Identifier: MIT -->
# 标识符

标识符可以由字母 (`a-zA-Z`)、数字 (`0-9`)、下划线 (`_`) 或者破折号 (`-`) 组成。
它们不能以数字或破折号开头（但可以以下划线开头）。
下划线和破折号会被规范化为破折号。这意味着这两个标识符是相同的：`foo_bar` 和 `foo-bar`。