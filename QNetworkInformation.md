QNetworkInformation 通过插件为网络相关信息提供了一个跨平台接口。

该类可以显示系统网络连接的当前状态，应用程序当前使用的传输介质（WiFi、Bluetooth 等）

> [! note] 
> **注意：** 由于该类是一个单例，同时也依赖于[QCoreApplication](https://doc.qt.io/qt-6/zh/qcoreapplication.html) ，因此 QNetworkInformation 应始终首先在与[QCoreApplication](https://doc.qt.io/qt-6/zh/qcoreapplication.html) 对象相同的线程中加载。这是因为该对象也将在该线程中销毁，而各种特定于后端组件可能依赖于在创建该对象时在同一线程中销毁。

> [! warning]
> 调用 `QNetworkInformation::instance()` 时并未进行初始化！[qnetworkinformation. cpp:767 - Codebrowser](https://codebrowser.dev/qt6/qtbase/src/network/kernel/qnetworkinformation.cpp.html#767)
## 检查网络是否在线

```cpp
#include <QNetworkInformation>

if (QNetworkInformation::loadDefaultBackend() && QNetworkInformation::loadBackendByFeatures(QNetworkInformation::Feature::Reachability)) {  
    auto info = QNetworkInformation::instance();  
  
    if (info->reachability() == QNetworkInformation::Reachability::Online) {  
        qDebug() << "online";  
    }  
    else {  
        qDebug() << "offline";  
    }
    
    // 关联信号，监听网络变化
    connect(info, &QNetworkInformation::reachabilityChanged, this,  
            [](QNetworkInformation::Reachability newReachability) {  
                if (newReachability == QNetworkInformation::Reachability::Online) {  
                    qDebug() << "online";  
                }  
            });  
}
```

> https://doc. qt. io/qt-6/qnetworkinformation. html
> 
> 