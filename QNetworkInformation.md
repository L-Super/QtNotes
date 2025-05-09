QNetworkInformation 通过插件为网络相关信息提供了一个跨平台接口。

该类可以显示系统网络连接的当前状态，应用程序当前使用的传输介质（WiFi、Bluetooth 等）

QNetworkInformation 是一个单例。
## 检查网络是否在线

```cpp
#include <QNetworkInformation>

auto info = QNetworkInformation::instance();
info->load(QNetworkInformation::Feature::Reachability);
info->reachability() == QNetworkInformation::Reachability::Online;
```