QVersionNumber 类包含具有任意数量段的版本号



| 成员函数                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| bool **[isNormalized](https://doc.qt.io/qt-6/qversionnumber.html#isNormalized)**() const | 如果版本号不包含任何尾随零，则返回 true，否则返回 false。    |
| bool **[isNull](https://doc.qt.io/qt-6/qversionnumber.html#isNull)**() const | 如果数值段为零，则返回 true，否则返回 false。                |
| bool **[isPrefixOf](https://doc.qt.io/qt-6/qversionnumber.html#isPrefixOf)**(const QVersionNumber &*other*) const | 如果当前版本号包含在其他版本号中，则返回 true，否则返回 false。 |
| int **[majorVersion](https://doc.qt.io/qt-6/qversionnumber.html#majorVersion)**() const | 返回主版本号，即第一个段。这个函数等效于 SegmentAt (0)。如果此 QVersionNumber 对象为 null，则此函数返回0。 |
| int **[microVersion](https://doc.qt.io/qt-6/qversionnumber.html#microVersion)**() const | 返回微版本号，即第三个段。这个函数等效于 SegmentAt (2)。如果此 QVersionNumber 对象不包含微版本号，则此函数返回0。 |
| int **[minorVersion](https://doc.qt.io/qt-6/qversionnumber.html#minorVersion)**() const | 返回次版本号，即第二段。这个函数等效于 SegmentAt (1)。如果此 QVersionNumber 对象不包含次版本号，则此函数返回0。 |
| QVersionNumber **[normalized](https://doc.qt.io/qt-6/qversionnumber.html#normalized)**() const | 返回等效的版本号，但删除了所有尾随零。                       |
| int **[segmentAt](https://doc.qt.io/qt-6/qversionnumber.html#segmentAt)**(qsizetype *index*) const | 返回索引处的段值。如果索引不存在，返回0。                    |
| qsizetype **[segmentCount](https://doc.qt.io/qt-6/qversionnumber.html#segmentCount)**() const | 返回存储在segments（）中的数量。                             |
| QList<int> **[segments](https://doc.qt.io/qt-6/qversionnumber.html#segments)**() const | 返回所有的数值段。                                           |
| QString **[toString](https://doc.qt.io/qt-6/qversionnumber.html#toString)**() const | 返回一个字符串，其中所有段都由句点（.）分隔。                |

---
静态函数

| 静态成员                                                     | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| QVersionNumber **[commonPrefix](https://doc.qt.io/qt-6/qversionnumber.html#commonPrefix)**(const QVersionNumber &*v1*, const QVersionNumber &*v2*) | 返回同时是 v1和 v2的父版本的版本号。 |
| int **[compare](https://doc.qt.io/qt-6/qversionnumber.html#compare)**(const QVersionNumber &*v1*, const QVersionNumber &*v2*) | 将v1与v2进行比较，并返回一个小于、等于或大于零的整数，具体取决于v1分别小于、等于还是大于v2。 |
| QVersionNumber **[fromString](https://doc.qt.io/qt-6/qversionnumber.html#fromString)**(QAnyStringView *string*, qsizetype **suffixIndex* = nullptr) | 从由句点(.)分隔的特殊格式的非负十进制数字字符串构造 QVersionNumber。 |

## 示例

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

### 当前版本号是否包含在其他版本号中

```cpp
QVersionNumber v1(1, 2);
QVersionNumber v2(1, 2, 3);
bool value = v1.isPrefixOf(v2);
qDebug() << value; 
// 输出：true
```





> https://doc.qt.io/qt-6/qversionnumber.html