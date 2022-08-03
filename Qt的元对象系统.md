Qt 采用元对象系统（meta-object system）为 QObject 派生类的对象提供非常详细的运行时类型信息以及数据成员的当前值信息。也就是说，给定一个 QObject 派生类对象，应用程序在运行阶段可以获取该对象所属类名称、父类名称、具有哪些成员函数、枚举类型、数据成员，应用程序甚至可以依据数据成员的名称，得到对应数据成员的当前取值。这些信息远比 C++的 RTTI 所提供的要丰富，使得应用程序能够获得一个 QObject 派生类对象的全部信息，这种设计模式也被称为 reflection（反射）模式。

Qt 并没有采用 C++的 RTTI 来实现元对象系统，它使用的是预处理工具 moc（meta-object compiler）。在定义 QObject 派生类时，我们调用宏 Q_OBJECT 定义一些成员函数，moc 分析该头文件，生成一些 C++源文件，这些源文件含有实现这些成员函数的代码，应用程序调用这些成员函数获取 QObject 派生类对象的详细信息。


## 类型信息的表示 
C++采用类 type_info 来描述一个类型的类型信息，而 Qt 采用类 QMetaObject 来描述 QObject 及其派生类的类型信息。类 QMetaObject 是 Qt 的元对象系统（Meta-object System）的核心。如图 17-3 所示，当满足一定的条件时，QObject 及其每个派生类会包含唯一一个 QMetaObject 的对象，用来描述 QObject 或其派生类的类型信息。