Qt 采用元对象系统（meta-object system）为 QObject 派生类的对象提供非常详细的运行时类型信息以及数据成员的当前值信息。也就是说，给定一个 QObject 派生类对象，应用程序在运行阶段可以获取该对象所属类名称、父类名称、具有哪些成员函数、枚举类型、数据成员，应用程序甚至可以依据数据成员的名称，得到对应数据成员的当前取值。这些信息远比 C++的 RTTI 所提供的要丰富，使得应用程序能够获得一个 QObject 派生类对象的全部信息，这种设计模式也被称为 reflection（反射）模式。

Qt 并没有采用 C++的 RTTI 来实现元对象系统，它使用的是预处理工具 moc（meta-object compiler）。在定义 QObject 派生类时，我们调用宏 Q_OBJECT 定义一些成员函数，moc 分析该头文件，生成一些 C++源文件，这些源文件含有实现这些成员函数的代码，应用程序调用这些成员函数获取 QObject 派生类对象的详细信息。


## 类型信息的表示 
C++采用类 type_info 来描述一个类型的类型信息，而 Qt 采用类 QMetaObject 来描述 QObject 及其派生类的类型信息。类 QMetaObject 是 Qt 的元对象系统（Meta-object System）的核心。如图所示，当满足一定的条件时，QObject 及其每个派生类会包含唯一一个 QMetaObject 的对象，用来描述 QObject 或其派生类的类型信息。

![](Qt.assets/Pasted%20image%2020220803094439.png)

以 QFileDialog 包含的对象 D 为例，所描述的主要信息如下：
（1）被描述类的名字，即字符串“QFileDialog”。这是最基本的信息，type_info 也能够做到。
（2）被描述类含有多少个成员函数，每个成员函数的原型是什么。
（3）被描述类的父类的类型信息。QFileDialog 的父类为 QDialog，描述 QDialog 类型信息的对象为 C。逻辑上，D 含有一个指向 C 的指针。一般意义上，对于任意两个具有父子关系的 QObject 派生类，它们的 QMetaObject 对象之间都存在这样的一个指针，形成图中从 D 出发，经由 C、B 抵达 A 的一条路径。这条路径可以被用来判断两个类之间是否具有继承关系。
（4）被描述类含有多少个枚举类型，每个枚举类型是如何定义的。
（5）被描述类的其他信息，比如类的设计者的姓名等。

## 继承关系的判断
给定 QObject 派生类的一个对象，我们有两种方法来判断该对象是否“具有”某个目标类型。
1. 类似于 C++的 dynamic_cast，我们使用 qobject_cast 来判断一个对象是否“具有”某个目标类型。该运算符的原型为：

```C++
DestType* qobject_cast<DestType*> ( QObject* p )
```

如果 p 所指的对象具有目标类型 DestType，返回一个类型为 `DestType*` 的指针，否则，返回一个空指针。`qobject_cast` 和 `dynamic_cast` 具有相似的功能，但是前者仅适用于 QObject 及其派生类，而后者适用于任何类型。由于目前绝大部分 C++编译器支持 RTTI，必需使用 qobject_cast 的场合很少。

2. 基类 QObject 提供了静态成员函数 inherits ()，用来判断一个对象是否具有某个目标类型。该函数的原型如下：
```C++
bool inherits(const char* className) const
```
该函数判断一个对象是否具有字符串参数 className 指定的那个类的类型。由于这个字符串参数是在程序运行阶段被确定的，这种方法比前一种方法更加灵活。

判断一个QObject派生类的对象是否“具有”某个类型：

```c++
QObject* pObject = new QFileDialog();
const QMetaObject* p = pObject->metaObject();
...
if (qobject_cast<QDialog *>(pObject))//①
	QMessageBox::information (0,"","The object has the type of QDialog");
if (pObject->inherits("QWidget"))//②
	QMessageBox::information (0,"","The object has the type of QWidget");
```

`QObject *`的指针指向一个`QFileDialog`的对象。行①使用`qobject_cast`将这个指针转换成类型为`QDialog *`，行②判断该对象是否具有`QWidget`的类型。

## 类型信息的获取

QObject 定义了一个虚函数 metaObject ()，它的每个派生类重载了该函数，以返回与该派生类对应的元对象。一旦得到一个元对象，我们可以调用 QMetaObject 的以下成员函数获取该元对象所描述类型的详细信息。

className () 返回被描述类的名字，superClass () 返回被描述类的父类的元对象，methodCount () 返回被描述类拥有多少个成员函数，而 method () 返回其中某个成员函数的具体信息，enumeratorCount () 返回被描述类拥有多少个枚举类型，而 enumerator () 返回其中某个枚举类型的具体信息。

如何获取 QFileDialog 的类型信息:

```c++
QObject* pObject = new QFileDialog();
const QMetaObject* p = pObject->metaObject();//①
QString str;
QTextStream textStream(&str);
textStream<< p->className()<<"inherits" << p->superClass()->className() << Qt::endl;//②
textStream<< "==methods==" << Qt::endl;
for(int i = p->methodOffset(); i < p->methodCount(); ++i)//③
    textStream<< p->method(i).methodSignature() << Qt::endl;
textStream<< "==enumerators ===" << Qt::endl;
for(int i = p->enumeratorOffset();i < p->enumeratorCount();++i)//④
    textStream<< p->enumerator(i).name() << Qt::endl;
QMessageBox::information(0,"",str);
```

首先创建一个 QFileDialog 对象，指向 QObject 的指针指向该对象。行①调用虚函数 metaObject ()，C++的多态性机制将确保 QFileDialog 的对应函数被调用，返回的元对象描述的是 QFileDialog 的类型信息。行②获取该类及其父类的名字信息。一个派生类的成员函数中，既有其本身定义的，也有从其基类继承的。行③的 methodOffset () 返回继承的成员函数的总数量。该行仅查询该序号之后成员函数的信息，也就是说，QFileDialog 本身定义的成员函数的信息。类似地，行④返回该类本身所定义的枚举类型的信息。

```
QFileDialog inherits QDialog
==methods==
fileSelected(QString)
filesSelected(QStringList)
currentChanged(QString)
directoryEntered(QString)
urlSelected(QUrl)
urlsSelected(QList<QUrl>)
currentUrlChanged(QUrl)
directoryUrlEntered(QUrl)
filterSelected(QString)
_q_pathChanged(QString)
_q_navigateBackward()
_q_navigateForward()
_q_navigateToParent()
_q_createDirectory()
_q_showListView()
_q_showDetailsView()
_q_showContextMenu(QPoint)
_q_renameCurrent()
_q_deleteCurrent()
_q_showHidden()
_q_updateOkButton()
_q_currentChanged(QModelIndex)
_q_enterDirectory(QModelIndex)
_q_emitUrlSelected(QUrl)
_q_emitUrlsSelected(QList<QUrl>)
_q_nativeCurrentChanged(QUrl)
_q_nativeEnterDirectory(QUrl)
_q_goToDirectory(QString)
_q_useNameFilter(int)
_q_selectionChanged()
_q_goToUrl(QUrl)
_q_goHome()
_q_showHeader(QAction*)
_q_autoCompleteFileName(QString)
_q_rowsInserted(QModelIndex)
_q_fileRenamed(QString,QString,QString)
==enumerators ===
ViewMode
FileMode
AcceptMode
Option
Options
```

## 对象数据的获取

Qt 的元对象系统不但允许应用程序在运行阶段查询一个对象的类型信息，它还允许查询一个对象的数据信息，即该对象具有多少个数据成员，每个数据成员的名字、类型、当前取值等信息。

具体地说，给定一个 QObject 派生类对象，与之对应的元对象提供了成员函数 propertyCount ()，返回该对象具有多少个数据成员。这个数字也包括该对象从其基类继承的数据成员。这个元对象还提供了成员函数 property ()，返回一个数据成员的名字、类型等信息。Qt 使用类 QMetaProperty 描述这些信息。最重要的是，在程序运行阶段，应用程序可以调用 QObject 的成员函数 property ()，依据一个数据成员的名字来查询该数据成员的当前取值。

获取一个 QObject 派生类对象的数据信息：

```c++
QFileDialog* p = new QFileDialog();
auto objToString = [](const QObject* obj){
    QString result;
    const QMetaObject *meta = obj->metaObject();
    for (int i=0; i < meta->propertyCount();++i){
        const QMetaProperty mp = meta->property(i);//①
        result += QString("%1 %2 %3;\n").arg(mp.typeName())//②
            .arg(mp.name())//③
            .arg (obj->property(mp.name()).toString());//④
    }
    return result;
};

QString str = objToString(p);
```

行①调用元对象的成员函数 property ()，获取第 i 个数据成员的信息，存放在一个 QMetaProperty 对象中。类 QMetaProperty 提供了一组丰富的成员函数，详细地描述一个数据成员的信息，行②、③调用了其中的两个，获取一个数据成员的类型、名字信息。
