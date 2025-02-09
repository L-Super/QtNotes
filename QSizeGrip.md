QSizeGrip 类为调整顶级窗口的大小提供了一个 `resize` 句柄。放在 QWidget 树的任何地方，用户可以用它来调整顶层窗口或任何 ` Qt::SubWindow ` 标志设置的 QWidget 的大小。它一般位于顶级窗口的右下角。

![](Qt.assets/Pasted%20image%2020250209230128.png)

QSizeGrip 类继承于QWidget ，并重新实现了 `mousePressEvent()` 和 `mouseMoveEvent()` 函数以提供调整大小功能，以及 ` paintEvent () ` 函数以渲染尺寸控件。

> [!note]
> QStatusBar 已经使用了该窗口部件，因此如果您有状态栏（例如，您使用的是QMainWindow ），则无需明确使用该窗口部件。QDialog 也是如此，您只需调用 `QDialog::setSizeGripEnabled()` 即可。



> https://doc.qt.io/qt-6/zh/qsizegrip.html