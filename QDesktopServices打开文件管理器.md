许多桌面环境提供的服务可以被应用程序用来执行常见的任务，比如打开一个网页，这种方式既一致又考虑到用户的应用程序首选项。

此类包含为这些服务提供简单接口的函数，这些接口指示服务是成功还是失败。

OpenUrl ()函数用于打开位于外部应用程序中任意 URL 的文件。对于与本地文件系统上的资源相对应的 URL (其中 URL 模式为“ file”) ，将使用一个合适的应用程序来打开该文件; 否则，将使用 Web 浏览器来获取和显示该文件。

用户的桌面设置控制某些可执行文件类型是打开以供浏览，还是执行。一些桌面环境配置为防止用户执行从非本地 URL 获得的文件，或者在执行之前请求用户的权限。

用默认浏览器（例如：Chrome）打开一个网址：

```c++
QDesktopServices::openUrl(QUrl("http://blog.csdn.net/liang19890820"));
```

使用文件资源管理器打开一个文件夹：

```c++
QDesktopServices::openUrl(QUrl("file:///D:/Program Files/Youdao"));
```



> [QDesktopServices](https://doc.qt.io/qt-5/qdesktopservices.html)