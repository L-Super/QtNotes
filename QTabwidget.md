QTableWidget 是 QTableView 的子类，主要的区别是 QTableView 可以使用自定义的数据模型来显示内容 (也就是先要通过 setModel 来绑定数据源)，而 QTableWidget 则只能使用标准的数据模型，并且其单元格数据是 QTableWidgetItem 的对象来实现的 (也就是不需要数据源，将逐个单元格内的信息填好即可)。

![](Qt.assets/Pasted%20image%2020221225201303.png)
