QtConcurrent 这是一个高级 API，构建于 QThreadPool 之上，它提供更高层次的 API，使所写的程序，可根据计算机的 CPU 核数，自动调整运行的线程数量。通常用于处理大多数通用的并行计算模式。



cmake配置：

```cmake
find_package(Qt6 REQUIRED COMPONENTS Concurrent)
target_link_libraries(mytarget PRIVATE Qt6::Concurrent)
```

## QtConcurrent:: run ()

注意，函数可能不会立即运行; 函数只有在线程可用时才会运行。

**代码示例**：

普通函数：

```cpp
QString func(int arg);
QFuture<QString> future = QtConcurrent::run(func, arg);
// 获取结果
QString result = future.result(); 
```

成员函数：

```cpp
QFuture<void> future = QtConcurrent::run(this, &Dialog::Func);
```

Lambda：

```cpp
QFuture<void> future = QtConcurrent::run([=](){
	qDebug() << QThread::currentThreadId() << QThread::currentThread();
});
```

