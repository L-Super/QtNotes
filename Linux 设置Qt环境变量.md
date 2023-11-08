编辑 `.bashrc` 文件

```bash
vim ~/.bashrc
```

追加以下内容：
```bash
# Qt ENV
export QTDIR=/home/Leou/Qt/5.15.2/gcc_64
export PATH=$QTDIR/bin:$PATH
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
export QT_PLUGIN_PATH=$QTDIR/plugins:$QT_PLUGIN_PATH
export QML2_IMPORT_PATH=$QTDIR/qml:$QML2_IMPORT_PATH
```