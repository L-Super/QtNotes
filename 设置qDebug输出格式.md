- [qDebug](https://runebook.dev/zh/docs/qt/qtglobal#qDebug) () 用于编写自定义调试输出。
- [qInfo](https://runebook.dev/zh/docs/qt/qtglobal#qInfo) () 用于信息性消息。
- [qWarning](https://runebook.dev/zh/docs/qt/qtglobal#qWarning) () 用于报告应用程序中的警告和可恢复错误。
- [qCritical](https://runebook.dev/zh/docs/qt/qtglobal#qCritical) ()用于写入严重错误消息和报告系统错误。
- [qFatal](https://runebook.dev/zh/docs/qt/qtglobal#qFatal) () 用于在退出前不久写入致命错误消息。

默认情况下，仅打印消息。但可以通过 `qSetMessagePattern(const QString &pattern)` 来包含其他信息。
## `qSetMessagePattern(const QString &pattern)`
允许调整 `qDebug ()`、 `qInfo ()`、 `qWarning ()`、` qCritical () `和` qFatal () `的输出。 `qCDebug ()`、 `qCInfo ()`、 `qCWarning ()` 和` qCCritical ()` 的类别记录输出也被格式化。


支持以下占位符：

| Placeholder                                | Description                                                  |
| ------------------------------------------ | ------------------------------------------------------------ |
| `%{appname}`                               | QCoreApplication::applicationName()                          |
| `%{category}`                              | Logging category                                             |
| `%{file}`                                  | 源文件路径                                                   |
| `%{function}`                              | Function                                                     |
| `%{line}`                                  | 源文件中的行                                                 |
| `%{message}`                               | 实际消息                                                     |
| `%{pid}`                                   | QCoreApplication::applicationPid()                           |
| `%{threadid}`                              | 当前线程的系统范围ID（如果可以获得）                         |
| `%{qthreadptr}`                            | 指向当前 QThread 的指针（ QThread::currentThread() 的结果）  |
| `%{type}`                                  | "debug" 、 "warning" 、 "critical" 或 "fatal"                |
| `%{time process}`                          | 消息的时间，自进程启动以来的秒数（标记 "process" 是文字）    |
| `%{time boot}`                             | 消息的时间，如果可以确定的话，以系统启动后的秒数为单位（标记 "boot" 是字面意思）。如果无法获取自启动以来的时间，则输出不确定（请参阅 QElapsedTimer::msecsSinceReference ()）。 |
| `%{time [format]}`                         | 消息发生时的系统时间，通过将 `format` 传递到 QDateTime::toString() 进行格式化。如果未指定格式，则使用 [Qt::ISODate](https://runebook.dev/zh/docs/qt/qt#DateFormat-enum) 格式。 |
| `%{backtrace [depth=N] [separator="..."]}` | 回溯，其帧数由可选 `depth` 参数指定（默认为 5），并由可选 `separator` 参数分隔（默认为 " |

还可以使用 `%{if-debug}` 、 `%{if-info}` 、 `%{if-warning}` 、 `%{if-critical}` 或 `%{if-fatal}` 后跟 `%{endif}` 对消息类型使用条件。仅当类型匹配时才会打印 `%{if-*}` 和 `%{endif}` 内部的内容。

最后，只有当类别不是默认类别时，才会打印 `%{if-category}` ... `%{endif}` 内的文本。

示例：

```cpp
QT_MESSAGE_PATTERN="[%{time yyyyMMdd h:mm:ss.zzz t} %{if-debug}D%{endif}%{if-info}I%{endif}%{if-warning}W%{endif}%{if-critical}C%{endif}%{if-fatal}F%{endif}] %{file}:%{line} - %{message}"
```

默认*pattern*是 `"%{if-category}%{category}: %{endif}%{message}" `

也可以在运行时通过设置 `QT_MESSAGE_PATTERN` 环境变量来更改；如果同时调用 `qSetMessagePattern()` 并设置 `QT_MESSAGE_PATTERN`，则环境变量优先。


>[! NOTE] 占位符 `category` 、 `file` 、 `function` 和 `line` 的信息仅记录在调试版本中。或者，可以显式定义 `QT_MESSAGELOGCONTEXT` 。有关更多信息，请参阅 [QMessageLogContext](https://runebook.dev/zh/docs/qt/qmessagelogcontext) 文档。


例子：

```cpp
qDebug() << "Widget()";

QString pattern = "[%{time yyyy-MM-dd h:mm:ss.zzz}] [%{type}] [%{file}:%{line}] %{message}";
qSetMessagePattern(pattern);
qDebug() << "Widget()2";
```

Output:

```
Widget()
[2024-05-22 18:14:01.646] [debug] [../qdemo1/widget.cpp:15] Widget()2
```





