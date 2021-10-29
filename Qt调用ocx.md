# Qt调用ocx

# ocx接口文件的生成

一般而言，安装好程序后就注册好ocx插件了。若未注册，用管理员身份打开cmd，进入到ocx所在目录，分32位和64位，路径不同64位系统存放32位文件路径`C:\Windows\SysWOW64`，64位文件`C:\Windows\System32`，执行注册命令：regsvr32 HWPenSign.ocx，注册完毕。

ocx对应有一个classid，可以通过oleview.exe软件查看

找到dumpcpp.exe，一般在qt安装目录下 `D:\Qt\Qt5.12.11\5.12.11\msvc2017\bin`，如果没有的话，可以在qt安装目录下找到dumpcpp.pro这个工程，编译生成。在该路径下打开cmd，执行命令，换成自己对应的id

```
dumpcpp.exe {44fbb90f-1eae-4b9b-ac58-8ebe18f79704}
```

若生成失败，一是ocx插件未注册成功，通过注册表查看。二是qt路径不对，32，64位路径都尝试一下。

生成成功后，会在当前路径下生成两个文件hwpensignlib.h和hwpensignlib.cpp，可复制到工程路径下，加入工程中。

> 参考资料 [Qt调用ocx插件接口，dumpcpp生成.h和.cpp接口文件](https://blog.csdn.net/tingy123/article/details/107676170)

同时在pro文件中，加入`QT += axcontainer`，可以更好地使用QAxobject类。

## ocx的调用

### 控件关联方式

1. Qt Designer方式

可直接在ui文件中拖动QAxWidget控件，该控件负责与ocx的交互。然后设置ocx绑定，可在ui里对该控件右键**设置控件**进行设置，但推荐采用代码方式完成绑定。

```c++
ui->axWidget->setControl("{44fbb90f-1eae-4b9b-ac58-8ebe18f79704}");
```

2. 代码方式

直接代码新建QAxWidget对象

### ocx控件函数的调用

```C++
ui->axWidget->dynamicCall("HWSetPenColor(QColor)",QColor(255,102,51));
ui->axWidget->dynamicCall("HWSetCtlFrame(int,QColor)",2,QColor(51,166,204));
auto res = ui->axWidget->dynamicCall("HWInitialize");
```





