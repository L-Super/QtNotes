- [qDebug](https://runebook.dev/zh/docs/qt/qtglobal#qDebug) () 用于编写自定义调试输出。
- [qInfo](https://runebook.dev/zh/docs/qt/qtglobal#qInfo) () 用于信息性消息。
- [qWarning](https://runebook.dev/zh/docs/qt/qtglobal#qWarning) () 用于报告应用程序中的警告和可恢复错误。
- [qCritical](https://runebook.dev/zh/docs/qt/qtglobal#qCritical) ()用于写入严重错误消息和报告系统错误。
- [qFatal](https://runebook.dev/zh/docs/qt/qtglobal#qFatal) () 用于在退出前不久写入致命错误消息。

默认情况下，仅打印消息。可以通过`qSetMessagePattern(const QString &pattern)`来包含其他信息。例如：