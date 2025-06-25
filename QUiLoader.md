QUiLoader 类可让应用程序在运行时使用 UI 文件中存储的信息或插件路径中指定的信息动态创建用户界面。

即可动态加载 .ui 文件。
```cpp
MyWidget::MyWidget(QWidget *parent)
    : QWidget(parent)
{
    QFile file(":/forms/myform.ui");
    if (!file.open(QFile::ReadOnly))
        qFatal("Cannot open resource file");

    QUiLoader loader;
    QWidget *myWidget = loader.load(&file, this);

    QVBoxLayout *layout = new QVBoxLayout;
    layout->addWidget(myWidget);
    setLayout(layout);
}
```
还有一个类似的 [QFormBuilder](https://doc.qt.io/qt-6/zh/qformbuilder.html) 类。

QFormBuilder 类通常用于嵌入Qt Widgets Designer 的自定义组件和应用程序。需要在运行时动态生成用户界面的独立应用程序则使用 [QUiLoader](https://doc.qt.io/qt-6/zh/quiloader.html) 类。

> [QUiLoader Class | Qt UI Tools](https://doc.qt.io/qt-6/quiloader.html)