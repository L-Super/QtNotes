# 通过Q_PROPERTY宏自定义属性
**任何可被识别的[Q_PROPERTY](property%20属性系统.md#Q_PROPERTY)都可以使用qproperty-语法设置。**

简单示例：

```c++

class Label : public QFrame
{
    ...
    Q_PROPERTY(QPixmap pixmap READ pixmap WRITE setPixmap)
};

//-----------------------------------------------------
auto pLabel = new Label;
pLabel->setObjectName("customLabel");
```

**通过QSS按照`qproperty-<property name>`语法的方式设置标签的属性**

```css
QLabel#customLabel {
        qproperty-pixmap: url(":/Images/logo.jpg");
}
```

项目代码：

```c++
//.h
class HSpinBoxPrivate;
class HSpinBox : public QWidget
{
    Q_OBJECT
    Q_PROPERTY(QIcon leftIcon READ leftIcon WRITE setLeftIcon)
public:
    explicit HSpinBox(QWidget *parent = nullptr);
    //相关的读写属性函数
    void setLeftIcon(const QIcon &icon);
    QIcon leftIcon() const;

};
```



```c++
//cpp
void HSpinBox::setLeftIcon(const QIcon &icon)
{
    Q_D(HSpinBox);
    d->setLeftIcon(icon);
}

QIcon HSpinBox::leftIcon() const
{
    Q_D(const HSpinBox);//注意这里需要加const
    return d->leftIcon();
}

```

创建`HSpinBox`对象后，设置对象名`setObjectName("HSpinBox");`即可在QSS文件中定义

```css
/*qss*/
QWidget#HSpinBox
{
    qproperty-leftIcon:url(":/resources/plus.png");
}
```

与直接调用 `setLeftIcon()` 函数效果一致，不过可以通过 QSS 文件进行修改。

> 注意！！！
> QPushButton 设置 Icon 图标经测试，也是通过此方法实现在 QSS 文件中设置图标的功能。
>
> ```css
> QPushButton{qproperty-icon: url (:/img/folder.png);}
> ```

# 通过property
通过设置属性的方式，利用属性的变化设置样式

[与QSS组合使用](动态属性.md#与QSS组合使用)
