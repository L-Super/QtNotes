# QButtonGroup

QButtonGroup 提供了一个可以放置按钮小部件的抽象容器。管理组中每个按钮的状态。

默认情况下，按钮组是独占的。按钮组中的按钮通常是可选择的 QPushButtons、 QCheckBoxes (通常用于非独占按钮组)或 QRadioButtons。如果创建独占按钮组，则应确保最初选中该组中的一个按钮; 否则，该组最初将处于未选中按钮的状态。

可以使用 addButton ()将按钮添加到组中，并使用 RemoveButton ()删除按钮。如果该组是独占的，则当前选中的按钮可用于 checkedButton ()。如果单击一个按钮，则发出 buttonClicks ()信号;

此外，QButtonGroup 可以在整数和按钮之间进行映射。可以使用 setId ()为按钮分配一个整数 id，并使用 id ()检索它。使用 checkedId ()可以获得当前选中按钮的 id，并且有一个重载信号 buttonClicks () ，它发出按钮的 id。**ID -1由 QButtonGroup 保留，表示“没有这样的按钮”**(所以添加按钮，id从-2、-3递增) 。映射机制的目的是简化用户界面中枚举值的表示。



## 函数

|                          | **[QButtonGroup](https://doc.qt.io/qt-5/qbuttongroup.html#QButtonGroup)**(QObject **parent* = nullptr) |
| ------------------------ | ------------------------------------------------------------ |
| virtual                  | **[~QButtonGroup](https://doc.qt.io/qt-5/qbuttongroup.html#dtor.QButtonGroup)**() |
| void                     | **[addButton](https://doc.qt.io/qt-5/qbuttongroup.html#addButton)**(QAbstractButton **button*, int *id* = -1) |
| QAbstractButton *        | **[button](https://doc.qt.io/qt-5/qbuttongroup.html#button)**(int *id*) const |
| QList<QAbstractButton *> | **[buttons](https://doc.qt.io/qt-5/qbuttongroup.html#buttons)**() const |
| QAbstractButton *        | **[checkedButton](https://doc.qt.io/qt-5/qbuttongroup.html#checkedButton)**() const |
| int                      | **[checkedId](https://doc.qt.io/qt-5/qbuttongroup.html#checkedId)**() const |
| bool                     | **[exclusive](https://doc.qt.io/qt-5/qbuttongroup.html#exclusive-prop)**() const |
| int                      | **[id](https://doc.qt.io/qt-5/qbuttongroup.html#id)**(QAbstractButton **button*) const |
| void                     | **[removeButton](https://doc.qt.io/qt-5/qbuttongroup.html#removeButton)**(QAbstractButton **button*) |
| void                     | **[setExclusive](https://doc.qt.io/qt-5/qbuttongroup.html#exclusive-prop)**(*bool*) |
| void                     | **[setId](https://doc.qt.io/qt-5/qbuttongroup.html#setId)**(QAbstractButton **button*, int *id*) |



## 信号

| void | **[buttonClicked](https://doc.qt.io/qt-5/qbuttongroup.html#buttonClicked)**(QAbstractButton **button*) |
| ---- | ------------------------------------------------------------ |
| void | **[buttonPressed](https://doc.qt.io/qt-5/qbuttongroup.html#buttonPressed)**(QAbstractButton **button*) |
| void | **[buttonReleased](https://doc.qt.io/qt-5/qbuttongroup.html#buttonReleased)**(QAbstractButton **button*) |
| void | **[buttonToggled](https://doc.qt.io/qt-5/qbuttongroup.html#buttonToggled)**(QAbstractButton **button*, bool *checked*) |
| void | **[idClicked](https://doc.qt.io/qt-5/qbuttongroup.html#idClicked)**(int *id*) |
| void | **[idPressed](https://doc.qt.io/qt-5/qbuttongroup.html#idPressed)**(int *id*) |
| void | **[idReleased](https://doc.qt.io/qt-5/qbuttongroup.html#idReleased)**(int *id*) |
| void | **[idToggled](https://doc.qt.io/qt-5/qbuttongroup.html#idToggled)**(int *id*, bool *checked*) |

## 示例

单击给定按钮时会发出此`buttonClicked`信号。

```cpp
connect(buttonGroup, QOverload<QAbstractButton *>::of(&QButtonGroup::buttonClicked),
    [=](QAbstractButton *button){ /* ... */ });
```

当单击具有给定 id 的按钮时，将发出此`idClicked`信号。

```cpp
QButtonGroup * group = new QButtonGroup(this);
group->addButton(ui->pushButton1, 0);
group->addButton(ui->pushButton2, 1);
connect(group, &QButtonGroup::idClicked, this, [this](int index) {
    qDebug() << "clicked index: " << index;
    ui->stackedWidget->setCurrentIndex(index);// 切换stackedWidget page
});
```