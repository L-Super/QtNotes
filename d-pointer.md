
通常情况下，与一个类密切相关的数据会被作为数据成员直接定义在该类中。然而，在某些场合下，我们会将这些数据从该类（被称为公类）分离出来，定义在一个单独的类中（被称为私类）。公类中会定义一个指针，指向私类的对象。在计算机的发展历史中，这种模式被称为pointer to implementation (**pimpl**)，handle/body或者cheshire cat。

Qt常将其命名为d_ptr或者d。Qt文档将其称为d-pointer。[Q_D和Q_Q指针](Qt.md#Q_D和Q_Q指针)