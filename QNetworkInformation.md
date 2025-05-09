QNetworkInformation 通过插件为网络相关信息提供了一个跨平台接口。

## 检查网络是否在线

```cpp
#include <QNetworkInformation>

auto info = QNetworkInformation::instance();
info->load(QNetworkInformation::Feature::Reachability);
info->reachability() == QNetworkInformation::Reachability::Online;
```