**QSignalSpy** 类是Qt 测试框架中的一个工具类，用于监听和记录信号的发射。

QSignalSpy 可以连接到任何对象的任何信号，并记录其发射情况。QSignalSpy 本身是一个QVariant 列表。信号的每次发射都会向列表追加一个项目，其中包含信号的参数。

```cmake
find_package(Qt6 REQUIRED COMPONENTS Test)
target_link_libraries(mytarget PRIVATE Qt6::Test)
```

记录了 `clicked()` 信号的所有信号发射：
```cpp
QCheckBox *box = ...;
QSignalSpy spy(box, SIGNAL(clicked(bool)));

// do something that triggers the signal
box->animateClick();

QCOMPARE(spy.count(), 1); // make sure the signal was emitted exactly one time
QList<QVariant> arguments = spy.takeFirst(); // take the first signal

QVERIFY(arguments.at(0).toBool() == true); // verify the first argument
```

捕获了一个来自自定义对象的信号：
```cpp
QSignalSpy spy(myCustomObject, SIGNAL(mySignal(int,QString,double)));

myCustomObject->doSomething(); // trigger emission of the signal

QList<QVariant> arguments = spy.takeFirst();
QVERIFY(arguments.at(0).typeId() == QMetaType::Int);
QVERIFY(arguments.at(1).typeId() == QMetaType::QString);
QVERIFY(arguments.at(2).typeId() == QMetaType::Double);
```



> [QSignalSpy](https://doc.qt.io/qt-6/qsignalspy.html)
