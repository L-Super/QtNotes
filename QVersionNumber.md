QVersionNumber 类包含具有任意数量段的版本号

### 版本号打印

```cpp
QVersionNumber version(1, 2, 3);
qDebug() << version; 
// 输出：1.2.3
```

### 转字符串

```cpp
QString versionStr = version.toString();
qDebug() << versionStr; 
// 输出："1.2.3"
```

### 子版本号

```cpp
QVersionNumber v(1, 2, 3);
qDebug() << v.majorVersion(); // 输出：1
qDebug() << v.minorVersion(); // 输出：2
qDebug() << v.microVersion(); // 输出：3
```

### 从字符串导入版本号

```cpp
QString str1 = "x.x.x"; // 导入无效的字符串
qDebug() << QVersionNumber::fromString(str1).toString(); 
// 输出: ""
QString str2 = "1.2.3"; // 导入有效字符串
qDebug() << QVersionNumber::fromString(str2).toString(); 
// 输出: "1.2.3"

QString str3("1.2.3-beta"); // 导入有效字符串
qDebug() << QVersionNumber::fromString(str3).toString(); 
// 输出: "1.2.3"

QString str4("1.2"); // 导入有效字符串
qDebug() << QVersionNumber::fromString(str4).toString(); 
// 输出: "1.2"

QString str5("1"); // 导入有效字符串
qDebug() << QVersionNumber::fromString(str5).toString(); 
// 输出: "1"
```

### 是否为空

```cpp
qDebug() << QVersionNumber(0, 0, 0).isNull(); 
// 输出：false
qDebug() << QVersionNumber().isNull(); 
// 输出：true
```

### 版本比较

```cpp
QVersionNumber v1(1, 0, 0);
QVersionNumber v2(1, 2, 3);
qDebug() << (v1 == v2); 
// 输出: false

QVersionNumber v1(1, 2);
QVersionNumber v2(1, 2, 0);
qDebug() << (v1 == v2); 
// 输出：false

QVersionNumber v1(1, 2);
QVersionNumber v2(1, 2, 0);
qDebug() << (v1.normalized() == v2.normalized()); 
// 输出：true
```

### 获取相同的子版本号

```cpp
QVersionNumber v1(1, 2);
QVersionNumber v2(1, 2, 0);
QVersionNumber v = QVersionNumber::commonPrefix(v1, v2);
qDebug() << v; 
// 输出：1.2
```

### 如果当前版本号包含在其他版本号中，则返回true，否则返回false

```cpp
QVersionNumber v1(1, 2);
QVersionNumber v2(1, 2, 3);
bool value = v1.isPrefixOf(v2);
qDebug() << value; 
// 输出：true
```





> https://doc.qt.io/qt-6/qversionnumber.html