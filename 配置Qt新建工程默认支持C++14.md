# 配置Qt新建工程默认支持C++14/17

在使用Qt Creator 新建工程时，默认配置仅支持C++11语法，如何修改默认配置支持C++14或C++17语法呢？

## 进入Qt安装目录

比如我的目录为`D:\Qt\Qt5.14.2`

进入目录`Tools\QtCreator\share\qtcreator\templates\wizards\projects`中

完整目录路径为`D:\Qt\Qt5.14.2\Tools\QtCreator\share\qtcreator\templates\wizards\projects`
![](Pasted%20image%2020211214130535.png)

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
![](Pasted%20image%2020211214130607.png)

## 配置Qt命令行(Qt Console Application)默认模板

打开 `consoleapp/file. pro` 文件

将默认的 

```
CONFIG += c++11 console
```

修改成

```
CONFIG += c++14 console
```

保存即可。

## 配置纯C++(Plain C++ Application)默认模板

打开 `plaincpp/file. pro` 文件

将默认的 

```
CONFIG += console c++11 
```

修改成

```
CONFIG += console c++14
```

保存即可。

经过如上的设置新建 Qt 窗口工程、Qt 命令行工程、纯 C++工程时就会默认支持 C++14/17 了。

## 新建工程测试

新建一个 Qt Widgets 工程，打开 Pro 文件，这里默认就是 C++14 了
![](Pasted%20image%2020211214130633.png)