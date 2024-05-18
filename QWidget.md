## 成员函数



| 成员函数                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| QPoint QWidget::mapFromGlobal(const QPoint &pos)             | 将全局屏幕坐标位置转换为widget坐标。                         |
| QPoint QWidget::mapFromParent(const QPoint &pos)             | 将父widget坐标位置转换为widget坐标。如果widget没有父类，则与 mapFromGlobal ()相同。 |
| QPoint QWidget::mapTo(const QWidget *parent, const QPoint &pos) | 将widget的坐标位置转换为父坐标系的位置。父widget不能是 nullptr，而且必须是调用widget的父widget。 |
| QPoint QWidget:: mapToGlobal (const QPoint &pos)             | 将widget坐标位置转换为全局屏幕坐标。例如，`mapToGlobal（QPoint(0,0)`将提供widget左上角像素的全局坐标。 |
| QPoint QWidget:: mapToParent (const QPoint &pos)             | 将 widget 的坐标位置转换为父部件中的坐标。如果 widget 没有父类，则与 mapToGlobal ()相同。 |
|                                                              |                                                              |

