单行文本编辑器。


## 输入控制
通过使用验证器（Validator）和输入掩码（InputMask）可以实现复杂的输入限制。

使用 `setMaxLength()` 可以定义文本的最大允许长度。使用 `inputMask()` 和 `setValidator()` 进一步限制文本内容。

`inputMask()` 设置 QLineEdit 的验证掩码。验证器可以代替掩码使用，也可以与掩码结合使用；
输入掩码是一个输入模板字符串。它可以包含以下元素：

| 屏蔽字符 | 定义在此位置有效的 [Category](https://doc.qt.io/qt-6/zh/qchar.html#Category-enum) 输入字符。 |
| -------- | ------------------------------------------------------------ |
| 元字符   | 各种特殊含义（详见下文）。                                   |
| 分隔符   | 所有其他字符都被视为不可更改的分隔符。                       |

参见：[inputMask](https://doc.qt.io/qt-6/zh/qlineedit.html#inputMask-prop)

`setValidator(const QValidator *v)` 将行编辑值的验证器设置为 v 。

行编辑的 `returnPressed()` 和 `editingFinished()` 信号只有在 v 将行编辑的内容验证为 Acceptable 时才会发出。在编辑过程中，用户可以将内容更改为任何 Intermediate 值，但不能将文本编辑为 v 验证为 Invalid 的值。

这样既可以限制编辑完成后存储的文本，又可以让用户有足够的自由将文本从一种有效状态编辑到另一种有效状态。

示例：
```cpp
// IPv4地址正则表达式验证
QString ipPattern = "\\b(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}"
                    "(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\b";
ui->lineEdit->setValidator(new QRegExpValidator(QRegExp(ipPattern), this);

// IPv4输入掩码
ui->lineEdit->setInputMask("000.000.000.000;_");

// 使用内置验证器
// 整数范围验证(10-120)
ui->intLineEdit->setValidator(new QIntValidator(10, 120, this));
```

>  [QLineEdit高级输入控制](https://mp.weixin.qq.com/s/b7vS4ZDjv5R5zYJ3xPEezA)