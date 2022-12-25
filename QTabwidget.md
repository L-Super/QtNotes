QTableWidget 是 QTableView 的子类，主要的区别是 QTableView 可以使用自定义的数据模型来显示内容 (也就是先要通过 setModel 来绑定数据源)，而 QTableWidget 则只能使用标准的数据模型，并且其单元格数据是 QTableWidgetItem 的对象来实现的 (也就是不需要数据源，将逐个单元格内的信息填好即可)。

![](Qt.assets/Pasted%20image%2020221225201303.png)

# 接口

## QTableWidget属性

1. **将表格变为禁止编辑**

在默认情况下，表格里的字符是可以更改的，比如双击一个单元格，就可以修改原来的内容，如果想禁止用户的这种操作，让这个表格对用户只读，可以这样：

```cpp
tableWidget->setEditTriggers(QAbstractItemView::NoEditTriggers);
```

`QAbstractItemView::EditTrigger`枚举：

```cpp
QAbstractItemView::NoEditTriggers  // 不能对表格内容进行修改
QAbstractItemView::CurrentChanged  // 任何时候都能对单元格修改
QAbstractItemView::DoubleClicked   // 双击单元格
QAbstractItemView::SelectedClicked  //  单击已选中的内容
QAbstractItemView::EditKeyPressed  // 当按下平台编辑键时，编辑开始
QAbstractItemView::AnyKeyPressed  //  按下任意键就能修改
QAbstractItemView::AllEditTriggers  // 以上条件全包括
```

2. **设置表格为整行选择**

```
tableWidget->setSelectionBehavior(QAbstractItemView::SelectRows); //整行选中的方式
```

`QAbstractItemView::SelectionBehavior`枚举:

```cpp
QAbstractItemView::SelectItems   // Selecting single items.选中单个单元格
QAbstractItemView::SelectRows    // Selecting only rows.选中一行
QAbstractItemView::SelectColumns // Selecting only columns.选中一列
```

3. **单个选中和多个选中的设置**

```cpp
tableWidget->setSelectionMode(QAbstractItemView::ExtendedSelection); //设置为可以选中多个目标
```

枚举：

```cpp
QAbstractItemView::NoSelection //不能选择
QAbstractItemView::SingleSelection //选中单个目标
QAbstractItemView::MultiSelection //选中多个目标

QAbstractItemView::ExtendedSelection 
QAbstractItemView::ContiguousSelection 的区别不明显，主要功能是正常情况下是单选，但按下Ctrl或Shift键后，可以多选
```

4. **表格表头的显示与隐藏**

对于水平或垂直方法的表头，可以用以下方式进行 隐藏/显示 的设置

```cpp
tableWidget->verticalHeader()->setVisible(false); //隐藏列表头 
tableWidget->horizontalHeader()->setVisible(false); //隐藏行表头
```

5. **对表头文字的字体、颜色进行设置**

```cpp
QTableWidgetItem *columnHeaderItem0 = tableWidget->horizontalHeaderItem(0); //获得水平方向表头的Item对象 
columnHeaderItem0->setFont(QFont("Helvetica")); //设置字体 
columnHeaderItem0->setBackgroundColor(QColor(0,60,10)); //设置单元格背景颜色 
columnHeaderItem0->setTextColor(QColor(200,111,30)); //设置文字颜色 
```
6. **排序**

```cpp
// 设置排序
ui->tableWidget->setSortingEnabled(true);
```

7. 设置表头显示模式

```cpp
// 设置拉伸模式
ui->tableWidget->horizontalHeader()->setSectionResizeMode(QHeaderView::Stretch);
```




## 单元格属性

1. **单元格设置字体颜色和背景颜色及字体**

```cpp
QTableWidgetItem *item = new QTableWidgetItem("Apple"); 
item->setBackground(QColor(0,60,10)); 
item->setForeground(QColor(200,111,100)); 
item->setFont(QFont("Helvetica")); 
tableWidget->setItem(0,3,item); 
```

如果需要对所有的单元格都使用这种字体，则可以使用 tableWidget->setFont(QFont("Helvetica"));

2. 设置单元格内文字的对齐方式

```cpp
item->setTextAlignment(Qt::AlignHCenter | Qt::AlignVCenter);  
```

3. **合并单元格效果的实现**

**************

```
tableWidget->setSpan(0, 0, 3, 1) //其参数为： 要改变单元格的 1行数 2列数 要合并的 3行数 4列数
```

4. **设置单元格的大小**

首先，可以指定某个行或者列的大小
***************

```
tableWidget->setColumnWidth(3,200); 
tableWidget->setRowHeight(3,60); 
```

还可以将行和列的大小设为与内容相匹配
```
tableWidget->resizeColumnsToContents(); 
tableWidget->resizeRowsToContents(); 
```

5. **获得单击单元格的内容**


通过itemClicked (QTableWidgetItem *)` 信号，就可以获得鼠标单击到的单元格，进而获得其中的文字信息
****************

```cpp
connect(tableWidget,SIGNAL(itemDoubleClicked(QTreeWidgetItem*,int)),this, SLOT( getItem(QTreeWidgetItem*,int)) );
```

6. **在单元格里加入控件**

QTableWidget不仅允许把文字加到单元格，还允许把控件也放到单元格中。比如，把一个下拉框加入单元格，可以这么做：
******

```cpp
QComboBox *comBox = new QComboBox(); 
comBox->addItem("Y"); 
comBox->addItem("N"); 
tableWidget->setCellWidget(0,2,comBox); 
```

