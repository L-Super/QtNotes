通常，qss 样式需要重新编译后才能看到效果。

把下面的代码放到 main () 函数里，当按下快捷键 `ctrl + l` 后就会加载一次 QSS，实现 QSS 的动态加载



```cpp
MainWindow mainWindow; // 程序的主窗口 
QShortcut *shortcut = new QShortcut(QKeySequence(Qt::CTRL + Qt::Key_L), &mainWindow);  <br>  <br>QObject::connect(shortcut, &QShortcut::activated, [] {  <br>    UiUtil::loadQss();  <br>});|
```