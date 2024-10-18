收集一组无参数信号，并使用与发送信号的对象相对应的整数、字符串或 Widget参数重新发出它们。请注意，在大多数情况下，可以使用 `lambda` 将自定义参数传递到槽，成本较低并且会简化代码。

该类支持使用 `setMapping()` 将特定字符串、整数、对象和 Widget与特定对象进行映射。然后，对象的信号可以连接到 `map()` 槽，该槽函数将发出一个信号（可以是 `mappedInt()`、 `mappedString()` 和 `mappedObject()`，其值与原始信号对象关联。之后可以使用 `removeMappings()` 删除映射。

示例：

```cpp
// ButtonWidget.h
class ButtonWidget : public QWidget
{
    Q_OBJECT

public:
    ButtonWidget(const QStringList &texts, QWidget *parent = nullptr);

signals:
    void clicked(const QString &text);

private:
    QSignalMapper *signalMapper;
};
```

```cpp
// ButtonWidget.cpp
ButtonWidget::ButtonWidget(const QStringList &texts, QWidget *parent)
    : QWidget(parent)
{
    signalMapper = new QSignalMapper(this);

    QGridLayout *gridLayout = new QGridLayout;
    for (int i = 0; i < texts.size(); ++i) {
        QPushButton *button = new QPushButton(texts[i]);
        // 将按钮的clicked()信号连接到QSignalMapper的map()槽
        connect(button, &QPushButton::clicked, signalMapper, &QSignalMapper::map);
        // 在QSignalMapper中创建从每个按钮到按钮文本的映射。
        signalMapper->setMapping(button, texts[i]);
        gridLayout->addWidget(button, i / 3, i % 3);
    }

	//
    connect(signalMapper, &QSignalMapper::mappedString,
            this, &ButtonWidget::clicked);

    setLayout(gridLayout);
}
```