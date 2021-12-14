# 配置Qt新建工程默认支持C++14/17

在使用Qt Creator 新建工程时，默认配置仅支持C++11语法，如何修改默认配置支持C++14或更新的C++17语法呢？

## 进入Qt安装目录

比如我的目录为`D:\Qt\Qt5.14.2`

进入目录`Tools\QtCreator\share\qtcreator\templates\wizards\projects`中

完整目录路径为`D:\Qt\Qt5.14.2\Tools\QtCreator\share\qtcreator\templates\wizards\projects`

## 配置Qt窗口(Qt Widgets Application)默认模板

打开`qtwidgetsapplication/project.pro`文件

将默认的 

```
CONFIG += c++11
```

修改成

```
CONFIG += c++14
```

保存即可。

## 配置Qt命令行(Qt Console Application)默认模板

打开consoleapp/file.pro文件

将默认的 

```
CONFIG += c++11 console
```

修改成

```
CONFIG += c++17 console
```

保存即可。

## 配置纯C++(Plain C++ Application)默认模板

打开plaincpp/file.pro文件

将默认的 

```
CONFIG += console c++11 
```

修改成

```
CONFIG += console c++17
```

保存即可。

经过如上的设置新建Qt窗口工程、Qt命令行工程、纯C++工程时就会默认支持C++14/17了。

## 新建工程测试

新建一个Qt Widgets工程，这里默认就是C++17了