
## QSS通用属性

## 控件类的QSS
### QCalendarWidget

代码方面也有一些需要注意的

```cpp
//设置一周的第一天
ui->calendarWidget->setFirstDayOfWeek(Qt::Sunday);
//设置为本地，才能使SingleLetterDayNames生效，显示为一，二形式
ui->calendarWidget->setLocale( QLocale::Chinese);//容易遇到的坑
ui->calendarWidget->setHorizontalHeaderFormat(QCalendarWidget::SingleLetterDayNames);
ui->calendarWidget->setVerticalHeaderFormat(QCalendarWidget::NoVerticalHeader);

//默认第一列和最后一列为红色，需要设置format改为黑色，QSS貌似不能实现。
QTextCharFormat format;
format.setForeground(QColor("#333333"));
ui->calendarWidget->setWeekdayTextFormat(Qt::Sunday, format);
ui->calendarWidget->setWeekdayTextFormat(Qt::Saturday, format);
```



```css
/*整体widget*/
QCalendarWidget QWidget { 
background-color:#FFFFFF;
 }

QCalendarWidget QToolButton {
    height: 60px;
    width: 150px;
    color: #333333;
    font-size: 24px;
    icon-size: 20px, 20px;
    background-color: #FFFFFF;
}

/*前后月按钮图标*/
QToolButton#qt_calendar_prevmonth,#qt_calendar_nextmonth{
	border:1px solid;
}
/*月份选择*/
QCalendarWidget QMenu {
    width: 150px;
    left: 20px;
    color: #333333;
    font-size: 18px;
    background-color: #FFFFFF;
}
QCalendarWidget QMenu::item:selected{
	color:red;
}
  
/* header row */
QCalendarWidget QWidget { 
alternate-background-color:#FFFFFF; 
}
  
/* normal days */
QCalendarWidget QAbstractItemView:enabled
{
    color: #333333; 
    background-color: #FFFFFF; 
    selection-background-color:#FFFFFF;
    selection-color: #3377FF;
	outline:1px solid #3377FF;
}
```

> https://doc.qt.io/qt-5/qcalendarwidget.html#setHeaderTextFormat
>
> https://www.cnblogs.com/yinsedeyinse/p/11597516.html
>
> https://qtdebug.com/qtbook-qss-calendar/

## 实用属性

+ 样式表的轮廓线属性
outline：绘制在对象边框上的轮廓

场景：一些控件有虚线框时需要去掉，或者需要加选中为边框时。

```css
font-size: 11pt;
padding: 2px 12px 2px 12px;
color: #FFFFFF;
border: 1px solid #3377FF;
background-color: qlineargradient(x1:0, y1:0, x2:0, y2:1, stop:0 #80AEFF, stop:1 #3377FF);
border-radius: 2px;
height:40px;
width: 200px;
outline:2px solid red;/*如图红色边框*/
```

![](Qt.assets/Pasted%20image%2020220929105403.png)

如图，默认会有虚线框。
![](Qt.assets/Pasted%20image%2020220929112102.png)

去掉可使用
```css
outline:none;
```

换为实线框可使用
```css
outline:1px solid #3377FF;
```
效果如图：
![](Qt.assets/Pasted%20image%2020220929112229.png)

