## 移动窗口
```cpp
//MainWindow.h
protected:
    void mousePressEvent(QMouseEvent* event) override;

    void mouseMoveEvent(QMouseEvent* event) override;

    void mouseReleaseEvent(QMouseEvent* /*event*/) override;
private:
    bool isMoving{false};
    QPoint pressPos;
```

```cpp
//MainWindow.cpp
void MainWindow::mousePressEvent(QMouseEvent *event)
{
	if(event->button() == Qt::MouseButton::LeftButton)
	{
		pressPos = event->pos();
		isMoving = true;
	}
}
void MainWindow::mouseMoveEvent(QMouseEvent *event)
{
	// 如果是全屏的情况下，则禁止移动
	if (window()->isMaximized() || window()->isFullScreen())  
	    return QWidget::mouseMoveEvent(event);
	if (isMoving) {
		QPoint diff = event->pos() - pressPos;

		window()->move(window()->pos() + diff);
	}
}
void MainWindow::mouseReleaseEvent(QMouseEvent *)
{
	isMoving = false;
}
```

如果仅限上方的某区域的高度可移动窗口。特别适合去掉系统标题栏，自定义标题栏的情况。

```cpp
void MainWindow::mousePressEvent(QMouseEvent* event)
{
    // 获取标题栏的宽度
    int borderWidth = customTitleBarWidth;
	// 当在标题栏的高度之内才能移动
    if (pressPos.y() < borderWidth)
        isMoving = true;
}
```


## QD 指针结构模板

```cpp
class MyClassPrivate;
class MyClass : public QObject {
  public:
    MyClass(QObject *parent = nullptr);
    virtual ~MyClass();
    void dummyFunc();

  private:
    QScopedPointer<MyClassPrivate> d_ptr;
    Q_DECLARE_PRIVATE(MyClass);
};

class MyClassPrivate {
  public:
    explicit MyClassPrivate(MyClass *parent) : q_ptr(parent) {}

    void foobar() {
      Q_Q(MyClass);
    }

  private:
    MyClass *const q_ptr;
    Q_DECLARE_PUBLIC(MyClass);
};

MyClass::MyClass(QObject *parent)
    : QObject(parent), d_ptr(new MyClassPrivate(this)) {}

MyClass::~MyClass() = default;

void MyClass::dummyFunc() {
    Q_D(MyClass);
    d->foobar();
}
```


## 程序重启
```cpp
qApp->quit();  
QProcess::startDetached(qApp->applicationFilePath(), QStringList());
```