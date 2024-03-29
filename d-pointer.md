
通常情况下，与一个类密切相关的数据会被作为数据成员直接定义在该类中。然而，在某些场合下，我们会将这些数据从该类（被称为公类）分离出来，定义在一个单独的类中（被称为私类）。公类中会定义一个指针，指向私类的对象。在计算机的发展历史中，这种模式被称为pointer to implementation (**pimpl**)，handle/body或者cheshire cat。

Qt常将其命名为d_ptr或者d。Qt文档将其称为d-pointer。[Q_D和Q_Q指针](Qt.md#Q_D和Q_Q指针)

## d-pointer的实现
实现d-pointer模式时还需要考虑其他一些因素。公类使用d_pointer可以访问私类的成员。有时，私类的成员函数需要访问公类的成员，比如公类中定义的信号。这需要在私类中定义一个逆向的指针，Qt将这个指针称为q-pointer。

```cpp
class MyClassPrivate; // 前置声明
class MyClass : public QObject {
public:
  MyClass(QObject *parent = nullptr);
  virtual ~MyClass();
  void dummyFunc();
signals:
  void dummySignal();

private:
  MyClassPrivate *const d_ptr;
  Q_DECLARE_PRIVATE(MyClass); // ①
};

class MyClassPrivate {
public:
  MyClassPrivate(MyClass *parent) : q_ptr(parent) {}

  void foobar() {
    Q_Q(MyClass); // 声明之后，即将q_func() = q，就可以直接使用q
    emit q->dummySignal();
  }

private:
  MyClass *const q_ptr;
  Q_DECLARE_PUBLIC(MyClass); // ②
};

MyClass::MyClass(QObject *parent)
    : QObject(parent), d_ptr(new MyClassPrivate(this)) {}

MyClass::~MyClass() {
  Q_D(MyClass); // 同Q_Q
  delete d;
}

void MyClass::dummyFunc() {
  Q_D(MyClass);
  d->foobar();
}
```

在这段代码中，公类MyClass定义了一个指针d_ptr来访问私类MyClassPrivate，而私类定义了一个指针q_ptr来访问公类。在这个简单的例子中，公类和私类本可以通过这两个指针非常方便地访问对方的数据。但是，在一些复杂的场合下，这两个指针并不是直接定义在公类、私类中的，而是被定义在它们的基类中。此时，就需要用到Qt定义的4个宏：[Q_DECLARE_PRIVATE与Q_DECLARE_PUBLIC](Qt.md#Q_DECLARE_PRIVATE与Q_DECLARE_PUBLIC)、[Q_D和Q_Q](Qt.md#Q_D和Q_Q指针)。

`Q_DECLARE_PRIVATE(MyClass)`作用是在公类中定义了一个成员函数d_func，返回一个指针，指向对应的私类。本例中，公类本身定义了一个指向私类的指针d_ptr，所以Qt应用程序可以直接使用这个指针，而不必调用d_func来获取这个指针。

总之，一方面，Qt在公类中定义了一个指针d_ptr指向私类，在宏Q_DECLARE_PRIVATE中定义了一个函数获取这个指针，用宏Q_D将这个指针重新命名为d，以便于访问私类对象。另一方面，Qt在私类中定义了一个指针q_ptr指向公类，在宏Q_DECLARE_PUBLIC中定义了一个函数获取这个指针，用宏Q_Q将这个指针重新命名为q，以便于访问公类对象。
