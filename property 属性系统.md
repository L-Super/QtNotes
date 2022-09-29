# Q_PROPERTY

声明一个属性，在继承QObject的类中使用Q_PROPERTY()宏。

```c++
Q_PROPERTY(type name
           (READ getFunction [WRITE setFunction] |
            MEMBER memberName [(READ getFunction | WRITE setFunction)])
           [RESET resetFunction]
           [NOTIFY notifySignal]
           [REVISION int]
           [DESIGNABLE bool]
           [SCRIPTABLE bool]
           [STORED bool]
           [USER bool]
           [CONSTANT]
           [FINAL])
```

+ 方括号中的是可选项，各选项之间使用空格隔开。
+ type：指定属性的类型，可以是 QVariant 支持的类型或用户自定义类型，若是枚举类型，还需使用 Q_ENUMS 宏对枚举进行注册，若是自定义类型，需要使用Q_DECLARE_METATYPE( Type )宏进行注册。
+ name：指定属性的名称。
+ READ getFunction：用于指定读取函数(读取属性的值)，其中 READ 表示读取，不可更改，getFunction
  用于指定函数名称。
  + 若没有指定 MEMBER 变量，则必须指定 READ 函数。
  + 通常，READ 函数是 const 的，READ 函数必须返回属性的类型或对该类型的引用。
+ WRITE setFunction：用于指定设置函数(设置属性的值)，其中 WRITE 表示写入，不可更改，setFunction
  用于指定函数名称。
  + 此函数必须只有一个参数，且返回值类型必须为 void。
  + 若仅为只读属性，则不需要指定 WRITE 函数。
+ MEMBER memberName：用于把指定的成员变量 memberName 设置为具有可读和可写性质，而无需创建
  READ 和 WRITE 函数。其中 MEMBER 表示成员，不可更改，memberName 表示类中的成员变量。
  + 若没有指定 READ 函数，则必须指定 MEMBER 变量。
+ RESET resetFunction：用于把属性重置为默认值，该函数不能有参数，且返回值必须为 void。其中 RESET 表示重置，不可更改，resetFunction 用于指定函数名称。
+ NOTIFY notifySignal：表示给属性关联一个信号。如果设置该项，则应指定该类中已经存在的信号，每当属性值发生变化时就会发出该信号。若使用 MEMBER 变量，则 NOTIFY 信号必须为零或一个参数，且参数必须与属性的类型相同，该参数将接受该属性的新值。
+ REVISION：设置版本号，默认为 0。
+ DESIGNABLE：用于设置属性在 GUI 设计工具的属性编辑器(例如 Qt 设计师)中是否可见，多数属性是可见的，默认值为 ture，即可见。该变量也可以指定一个返回布尔值的成员函数替代 true 或 false。
+ SCRIPTABLE：设置属性是否可被脚本引擎访问，默认为 true
+ STORED：设置在保存对象的状态时是否必须保存属性的值。大多数属性此值为 true
+ USER：设置属性是否为可编辑的属性，每一个类只有一个 USER 属性(默认值为
  false)。比如 QAbstractButton::checked 是用户可编辑的属性
+ CONSTANT：表明属性的值是常量，常量属性不能有 WRITE 函数和 NOTIFY 信号。
  对于同一个对象实例，每一次使用常量属性的 READ 函数都必须得到相同的值，但
  对于类的不同实例，这个值可以不同。
+ FINAL：表示属性不能被派生类重写。

属性类型可以是QVariant支持的任何类型，或者是用户定义的类型。

一般形式：

```
Q_PROPERTY(QDate date READ getDate WRITE setDate)
```



示例：

```c++
class A:public QObject{ 
    Q_OBJECT
public:
//通常，若属性名为 a，则相应的读取函数通常命名为 geta，设置函数命名为 seta
    
//声明一个类型为 int，名称为 a 的属性，并使用函数 getA 读取属性值，使用函数 setA 设置属性值。
Q_PROPERTY(int a READ getA WRITE setA)
//声明一个类型为 int，名称为 b 的属性，并把该属性与成员变量 m_b 相关联，该属性未设置存取函数。
Q_PROPERTY(int b MEMBER m_b)
//声明一个只读属性 c，本例没有设置该属性值的函数，因此该属性是只读的。
Q_PROPERTY(int c READ getC)
    
	//在存取函数中可把属性与成员变量相关联。
	int getA() {return m_a;} //属性 a 的读取函数。
	void setA(int i) {m_a=i;} //属性 a 的设置函数。
	int getC() {m_c=3; return m_c;} //成员变量可以不与属性相关联，也可直接返回数值 3。从此处可看到，属性可以不与数据成员相关联。
	int m_a,m_b; //属性若命名为 a，则与其相对应的成员变量习惯上应命名为 m_a。
private:
    int m_c; 
};


```

```c++
//cpp源文件内容
int main(int argc, char *argv[])
{ 
    A ma;
	//像普通成员变量一样存取属性值
	ma.m_a=1; 
    cout<<ma.m_a<<endl; //输出 1
	//因为属性 b 没有存取函数，本例暂时只使用普通成员变量的方式存取该属性值。
 	ma.m_b=2; 
    cout<<ma.m_b<<endl; //输出 2
	ma.setA(4);//使用属性 a 的设置函数修改属性值。
	cout<<ma.getA()<<endl; // 输出 4，使用属性 a 的读取函数读取属性值。
	cout<<ma.getc()<<endl; //输出 3，属性 c 是只读的，只能通过他的读取函数访问其值，因为没有设置函数，因此无法改变属性 c 的值。
	return 0; 
}
```

摘自QWidget类的典型属性声明：

```c++
Q_PROPERTY(bool focus READ hasFocus)
Q_PROPERTY(bool enabled READ isEnabled WRITE setEnabled)
Q_PROPERTY(QCursor cursor READ cursor WRITE setCursor RESET unsetCursor)
```



> 《Qt 5.10 GUI完全参考手册》
>
> [Qt 之属性系统](https://blog.csdn.net/liang19890820/article/details/52022714)
> [Q_PROPERTY-原始属性](https://www.cnblogs.com/itrena/p/5938312.html)
> [Q_PROPERTY-自定义属性](https://www.cnblogs.com/itrena/p/5938311.html)
