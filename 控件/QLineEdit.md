单行文本编辑器。


## 限制文本
使用 `setMaxLength()` 可以定义文本的最大允许长度。使用 `inputMask()` 和 `setValidator()` 进一步限制文本内容。

`inputMask()` 设置 QLineEdit 的验证掩码。验证器可以代替掩码使用，也可以与掩码结合使用；
输入掩码是一个输入模板字符串。它可以包含以下元素：

| 屏蔽字符 | 定义在此位置有效的[Category](https://doc.qt.io/qt-6/zh/qchar.html#Category-enum) 输入字符。 |
| -------- | ------------------------------------------------------------ |
| 元字符   | 各种特殊含义（详见下文）。                                   |
| 分隔符   | 所有其他字符都被视为不可更改的分隔符。                       |

参见：[inputMask](https://doc.qt.io/qt-6/zh/qlineedit.html#inputMask-prop)

`setValidator()`