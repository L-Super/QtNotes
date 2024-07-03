解析域名的IP 或者 DNS 解析

### 方法一

使用`QHostInfo::fromName`静态函数。

**注意：在查找期间阻塞**

```cpp
QString hostname = "baidu.com";
// 使用QHostInfo进行解析
QHostInfo info = QHostInfo::fromName(hostname);
// 检查解析是否成功
if (info.error() == QHostInfo::NoError)
{
    // 获取第一个IP地址
    QList<QHostAddress> addresses = info.addresses();
    if (!addresses.isEmpty())
    {
        qDebug() << "IP address:" << addresses;
    }
    else
    {
        qDebug() << "No IP addresses found!";
    }
}
else
{
    qDebug() << "Error:" << info.errorString();
}
```

### 方法二

使用`QHostInfo::lookupHost`静态函数

```cpp
QHostInfo::lookupHost("api.yunxiu.meitu.com", [](const QHostInfo &host) {
    if (host.error() != QHostInfo::NoError)
    {
        qDebug() << "Lookup failed:" << host.errorString();
        return;
    }

    const auto addresses = host.addresses();
    for (const QHostAddress &address : addresses)
        qDebug() << "Found address:" << address.toString();
});
```

### 方法三

使用QDnsLookup进行解析。QDnsLookup 使用操作系统提供的机制来执行 DNS 解析操作。

相比QHostInfo，它可以指定用于进行DNS查找的服务器。

```cpp
QDnsLookup *dns = new QDnsLookup();
QDnsLookup::connect(dns, &QDnsLookup::finished, [dns] {
    if (dns->error() != QDnsLookup::NoError)
    {
        qWarning("DNS查找失败");
        dns->deleteLater();
        return;
    }

    foreach (auto &record, dns->hostAddressRecords())
    {
        qDebug() << record.name() << record.value();
    }

    dns->deleteLater();
});

dns->setType(QDnsLookup::A);
dns->setNameserver(QHostAddress("8.8.8.8")); // 可指定 host
dns->setName("baidu.com");
dns->lookup();
```