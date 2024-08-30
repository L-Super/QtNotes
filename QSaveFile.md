`QSaveFile` 是一个I/O设备，用于写入文本和二进制文件，如果写入操作失败，不会丢失现有数据。

写入时，内容将被写入临时文件，如果没有错误发生， `commit()`会将其移动到最终文件。这可以确保在写入时发生错误时最终文件中的数据不会丢失，并且最终位置上不会出现部分写入的文件。将整个文档保存到磁盘时始终使用 QSaveFile。

QSaveFile在写入时自动检测错误，例如满分区情况， `write()`无法写入所有字节。它会记住发生了错误，并丢弃 `commit()`中的临时文件。

与 `QFile` 非常相似，文件是用 ` open () ` 打开的。通常使用 `QDataStream` 或 `QTextStream` 读写数据，但也可以调用 `QIODevice` 继承的函数 `read()`、 `readLine()`、 ` readAll () `、 ` write () `。

与 `QFile` 不同，不允许调用 `close()`。 `commit()` 取代了它。如果未调用 `commit()` 并且 `QSaveFile` 实例被销毁，则临时文件将被丢弃。

要因应用程序错误而中止保存，请调用 `cancelWriting()`，这样即使稍后调用 `commit()` 也不会保存。

> [QSaveFile](https://runebook.dev/zh/docs/qt/qsavefile)