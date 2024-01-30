QSignalBlocker 信号阻塞器。

类似与mutex互斥锁。

 可以用在任何需要调用 blockSignals() 的地方。它在构造函数中阻止信号，并在析构函数中将状态重置为构造函数运行之前的状态。

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

