QSignalBlocker 信号阻塞器

与 mutex 互斥锁类似，不过是对信号的互斥。

 可以用在任何需要调用 `blockSignals()` 的地方。它在构造函数中阻止信号，并在析构函数中将状态重置为构造函数运行之前的状态。（RAII 机制）

```cpp
{
  const QSignalBlocker blocker(someQObject);
  // no signals here
}
```

因此相当于

```rust
const bool wasBlocked = someQObject->blockSignals(true);
// 这里没有信号
someQObject->blockSignals(wasBlocked);
```

示例：
```cpp
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    auto timer  = new QTimer();
    connect(timer, &QTimer::timeout, this, []{
        qDebug()<<"timer"<<QDateTime::currentDateTime();
    });
    timer->start(1000);
    
    connect(ui->pushButton, &QPushButton::clicked, this, []{
        QSignalBlocker blocker(timer);
        QEventLoop eventloop;
        QTimer::singleShot(5000, &eventloop, SLOT(quit()));
        eventloop.exec();
        qDebug()<<"过了5秒";
    });
}
```
当按钮点击事件发生后，timer 计时器会被阻塞五秒再打印。