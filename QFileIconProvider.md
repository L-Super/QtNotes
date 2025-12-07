QFileIconProvider 可提供文件和目录的图标。

| 函数                                 | 描述                                |
| ------------------------------------ | ----------------------------------- |
| QIcon icon(IconType type)            | 获取IconType类型对应的图标          |
| QIcon icon(const QFileInfo & info)   | 获取QFileInfo文件信息对应的图标     |
| QString type(const QFileInfo & info) | 获取QFileInfo文件信息对应的类型描述 |

示例：


```cpp
QIcon GetAppIcon(const QString& appPath) {  
  QFileIconProvider iconProvider;  
  return iconProvider.icon(QFileInfo(appPath));  
}
```