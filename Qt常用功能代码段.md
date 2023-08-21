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

