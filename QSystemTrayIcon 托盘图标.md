为应用程序在系统托盘中提供一个图标。

添加系统托盘项，首先，需要创建一个QSystemTrayIcon对象，并调用setContextMenu()为图标提供上下文菜单，然后，调用show()使其在系统托盘中可见。

## 信号

| 信号                                                         | 描述                                                      |
| ------------------------------------------------------------ | --------------------------------------------------------- |
| void **[activated](https://doc.qt.io/qt-6/qsystemtrayicon.html#activated)**(QSystemTrayIcon::ActivationReason *reason*) | 当用户激活系统托盘图标时发出此信号                        |
| void messageClicked()                                        | 当用户单击使用 showMessage ()显示的消息时，将发出此信号。 |

### 枚举QSystemTrayIcon::ActivationReason

此枚举描述了系统托盘被激活的原因。

| 常量                         | 值   | 描述                     |
| ---------------------------- | ---- | ------------------------ |
| QSystemTrayIcon::Unknown     | 0    | 未知原因                 |
| QSystemTrayIcon::Context     | 1    | 系统托盘的上下文菜单请求 |
| QSystemTrayIcon::DoubleClick | 2    | 双击系统托盘             |
| QSystemTrayIcon::Trigger     | 3    | 单击系统托盘             |
| QSystemTrayIcon::MiddleClick | 4    | 鼠标中键点击系统托盘     |

### 枚举QSystemTrayIcon::MessageIcon

此枚举描述了显示气球消息时所显示的图标。

| 常量                         | 值   | 描述                   |
| ---------------------------- | ---- | ---------------------- |
| QSystemTrayIcon::NoIcon      | 0    | 无图标显示             |
| QSystemTrayIcon::Information | 1    | 一个信息图标显示       |
| QSystemTrayIcon::Warning     | 2    | 一个标准的警告图标显示 |
| QSystemTrayIcon::Critical    | 3    | 一个严重的警告图标显示 |

## 示例

```cpp
// 设置菜单项
auto trayMenu = new QMenu();
auto aboutAction = new QAction("关于");
auto exitAction = new QAction("退出");

aboutAction->setIcon(QIcon(":/resources/images/info.svg"));
exitAction->setIcon(QIcon(":/resources/images/power.svg"));

trayMenu->addAction(aboutAction);
trayMenu->addSeparator();
trayMenu->addAction(exitAction);

connect(exitAction, &QAction::triggered, qApp, &QApplication::quit);

connect(aboutAction, &QAction::triggered, this, [this]
{
    AboutDialog aboutDialog;
    aboutDialog.exec();
});

// 设置托盘图标
auto trayIcon = new QSystemTrayIcon(this);
trayIcon->setIcon(QIcon(":/resources/images/clipboard2.svg"));
// 在右键时，弹出菜单。
trayIcon->setContextMenu(trayMenu);
trayIcon->show();
// show()之后才生效
trayIcon->setToolTip("QClipboard");
// 弹出气泡通知消息
trayIcon->showMessage("QClipboard 剪贴板", "已隐藏至系统托盘");

connect(trayIcon, &QSystemTrayIcon::activated, this, [](QSystemTrayIcon::ActivationReason reason){
    switch (reason) {
	case QSystemTrayIcon::Trigger:
	case QSystemTrayIcon::DoubleClick: {
		this->showNormal();
	}
		break;

	default:
		break;
	}
});
```

需要注意的是，QSystemTrayIcon 必须设置图标。

`setToolTip()` 和 `showMessage()` 需要在 `show()` 之后调用，暂不清楚是否平台差异，可能和设置完之后就 `show()` 有关。



> [QSystemTrayIcon Class](https://doc.qt.io/qt-6/qsystemtrayicon.html)
