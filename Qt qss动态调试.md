通常，qss 样式需要重新编译后才能看到效果。使用以下方法可使用快捷键刷新即可预览效果。



在程序运行的时候修改 QSS，当按下快捷键 `ctrl + l` 后就会加载一次 QSS，实现 QSS 的动态加载，然后就能看到修改后的效果

```cpp
//main.cpp
void LoadQss() {
    // 从配置中读取 QSS 的文件名，QSS 可以分类放在多个文件里
    QStringList qssFileNames = Singleton<Config>::getInstance().getQssFiles();

    // 需要把所有文件中的 QSS 合并在一起
    QString qss;

    foreach (QString name, qssFileNames) {
        qDebug() << QString("=> 加载 QSS 文件 %1").arg(name);

        QFile file(name);
        if (!file.open(QIODevice::ReadOnly)) {
            qDebug() << QString("打开 QSS 文件 %1 失败");
        }

        qss.append(file.readAll()).append("\n");
    }

    if (!qss.isEmpty()) {
        // qDebug() << qss;
        qApp->setStyleSheet(qss);
    }
}
int main()
{
    //...
    MainWindow mainWindow; 
    QShortcut *shortcut = new QShortcut(QKeySequence(Qt::CTRL + Qt::Key_L), &mainWindow);
    QObject::connect(shortcut, &QShortcut::activated, [] { loadQss(); });
    //...
}
```