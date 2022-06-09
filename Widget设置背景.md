# 常规设置背景图片的方式
## 方式一

在QWidget构造函数中添加代码

设置背景图片：

```c++
QPalette palette;
palette.setBrush(QPalette::Window, QBrush(QPixmap(":/images/bg.svg")));
// palette.setBrush(QPalette::Window, QBrush(QPixmap(":/images/bg.svg").scaled(this->size())));
setPalette(palette);
```

设置背景颜色：

```c++
QPalette palette;
//设置主窗口背景颜色
palette.setColor(QPalette::Window, QColor(255, 150, 30));
setPalette(palette);
```

## 方式二

使用样式表或者是采用QSS文件

```c++
setStyleSheet("QMainWindow#MainWindow{border-image: url(:/images/bg.svg);}");
```

需要注意不加`QMainWindow#MainWindow`限定是否对其他QWidget有影响，因为子窗体会继承父窗体的属性。

> background-image：按像素显示图片。图片比控件大会裁剪掉后面多余的部分，图片比控件小，多出的区域再填图片。
>
> border-image：可根据窗体大小来自动缩放图片填充

## 方式三

通过重写paintEvent事件，来设置背景图片和背景颜色

```c++
void MainWindow::paintEvent(QPaintEvent *)
{
    QPainter painter(this);
    QPixmap pixmap(":/image/background.png");
    painter.drawPixmap(this->rect(),pixmap);
}
```

# 子Widget背景设置

有时Widget里有一个Widget2，两者都需要设置背景，此时使用QSS样式表会失效，显示不正常。或者从QWidget子类化一个MyWidget。

为了能够使用样式表，需要重写paintEvent事件。

```c++
void MyWidget::paintEvent(QPaintEvent *event)
{
    Q_UNUSED(event);
    QStyleOption opt;
    opt.init(this);
    QPainter p(this);
    style()->drawPrimitive(QStyle::PE_Widget, &opt, &p, this);
}
```

