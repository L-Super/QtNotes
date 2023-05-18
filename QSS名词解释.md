
一、选择器（selector）
---------------

意思是：选择特定的类，一般为一个可以定制样式表的 Qt 类，所谓的选择器可以理解为 CSS 中的选择器，他指定了一类部件进行设计。下图总结了最有用的选择器。

| 选择器 | 实例 | 可以匹配的窗口部件 |
| --- | --- | --- |
| 通配符选择器 | * | 匹配所有控件 |
| 类型选择器 | QPushButton | 通过控件类型来匹配控件(包含子类) |
| 类选择器 | .QPushButton | 通过控件类型来匹配控件(不包含子类) |
| ID选择器 | QPushButton#okButton | 通过 objectName 来匹配控件 |
| 属性选择器 | QPushButton\[flat=”false”\] | 通过属性值来匹配控件 |
| 后代选择器 | QWidget QPushButton | 通过父控件的(直接或者间接)子控件来筛选控件 |
| 子选择器 | QWidget > QPushButton | 通过父控件的(直接)子控件来筛选控件，间接的不行 |

思维导图如下：

![](https://s2.loli.net/2022/03/22/MWYT9N7qPbhfuVF.png)

![](https://s2.loli.net/2022/03/22/bxCtmv4XRSB28Hf.png)

  

二、辅助控制器(sub-control)
--------------------

辅助控制器一词是相对于选择器存在的，可以理解为我们选择了一个部件，例如一个 QcheckBox，这个部件它分为两个部分，文本部分和可以点击的小窗口的部分，如下图所示。而这个可点击的小窗口部分我们要单独的设置，就要再次分离出来，就需要`::indicator`（QCheckBox 有这个辅助控制器）来设置，例子如下：

```css
QCheckBox::indicator{
	width:20px;
	height:20px;
}
```

辅助控制器是用双冒号进行指定。如果没有`::indicator`那么我们这个小例子将是对整个 QCheckBox 设置的，使用了辅助控制器的时候就自动分离出这个小窗口，对小窗口进行设置。不同的选择器有不同的辅助控制器，具体可查看下表。

| 辅助控制器 | 说明 |
| --- | --- |
| ::add-line | 在 QScrollBar 中添加一行的按钮 |
| ::add-page:branch | 在 QScrollBar 中添加页的按钮 |
| ::branch | QTreeView 的分支 |
| ::chunk | QProgressBar 的进度条里的块（里面的进度条可以变成一块一块的增加，而不是整体都是一种颜色) |
| ::colse-button | QDockWidget 和 QTabBar 标题栏上的关闭按钮 |
| ::corner | 在 QAbstractScrollArea 两个滚动条之间的位置 |
| ::drwn-arrow | QComboBox、QHeaderView (排序时需要)、QScrollBar、QSpinBox 的向下箭头 |
| ::up-button | QScrollBar、QSpinBox 的向上按钮 |
| ::down-button | QScrollBar、QSpinBox 的向下按钮 |
| ::drop-down | QComboBox 展开时 |
| ::float-button | QDockWidget 标题栏上的浮动按钮 |
| :groove | Qslider的槽 |
| :indicator | QAbstractitemView、QCheckBox、QRadioButton、可点击的 QMenu 的 item、可点击的 QGroupBox 的指示符 |
| ::handle | QScrollBar、QSplitter、QSlider 的滑块 |
| ::icon | QAbstractitemView 和 QMenu 的图标 |
| ::item | QAbstractitemView、QMenuBar、QMenu、QStatusBar 的单独的一项 |
| ::left-arrow | QScrollBar 的向左的箭头 |
| ::left-corner | QTabWidget 的左侧 |
| ::menu-arrow | 菜单里 QToolButton 箭头 |
| ::menu-button | 工具栏上的按钮 |
| ::menu-indicator | 菜单里的 QPushButton 指示符 |
| ::right--arrow | QMenu 或者是 QScrollBar 的右侧箭头 |
| ::pane | QTabWidget 去掉标题的框架 |
| ::right-corner | QTabWidget 的右侧 |
| ::scroller | QMenu 和 QTabBar 因为界面大小布局左右调试的滚动按钮 |
| ::section | QHeaderView 的表头横向和纵向 |
| ::separator | QMainWindow 和 QMenu 的分离器(就是一个主窗口被分割成几个小的区域的线，QMenu 里是 item 的分离线) |
| ::sub-line | QScrollBar 内容减少方向的按钮 |
| ::sub-page | QScrollBar 减少一页的按钮，在滑块与减少一行 sub-line 之间 |
| ::tab | QTabBar 和 QToolBox 的一个页选项 |
| ::tab-bar | 一个 QTabWidget 的 tab 按钮，设置 tabs 一般用 ::tab |
| ::tear | TabBar 的指示符 |
| ::tearoff | QMenu 的指示符 |
| ::text | QAbstractitemView 的内容 |
| ::title | QGroupBox 和 QDockWidget 的标题 |
| ::up-arrow | QHeaderView（排序时）、QScrollBar、QSpinBox 向上按钮箭头 |

三、伪状态（ pseudo-states)
---------------------

除了辅助控制器对一个部件的分离，样式表还可以根据窗口部件的各个状态来设置窗口。例如 hover 表示鼠标划过时的状态，例子如下：

``` css
QCheckBox:hover{
	color: red;
}
```

伪状态列表如下：

| 状态 | 说明 |
| --- | --- |
| :checked | 按钮已选中 |
| :unchecked | 按钮未被选中 |
| :hover | 鼠标划过窗口部件时的状态 |
| :pressed | 控件被按下 |
| :focus | 窗口部件有输入焦点 |
| :disabled | 禁用窗口部件 |
| :enabled | 启用窗口部件 |
| :indeterminate | 按钮部分被选中的状态 |
| :on | 控件处于 on 状态 |
| :off | 控件处于 off 状态 |
| :active | 当前活动的窗口 |
| :flat | 没有突起的部件 |
| :exclusive | 表示按钮组设置为单选，只能选择一个的状态。例如菜单栏的选项 |
| :non-exclusive | 不能单选的，菜单来就是单选的 |
| :default | 默认的状态 |
| :horizontal | 部件是横向的。 |
| :editabled | QComboBox 可以编辑的 |
| :edit-focus | 那种可编辑的控件，比如文本框，当它正在编辑的时候，就是 QStyle: :State_HasEditFocus 状态 |
| :no-frame | 这个部件是没有 frame 的，例如 QLineEdit 和 QSpinBox |
| :closable | items 是可以关闭的，例如 QDockWidget 有一个 QDockWidget::DockWidgetClosable 的功能 |
| :floatable | 部件是可浮动，例如 QDockWidget |
| :movable | 这个部件可以移动，例如 QDockWidget |
| :closed | (open 相对的)窗口位于关闭或者销毁的状态，例如QTreeView没有打开时的状态 |
| :has-children | Item有子目录的,例如 QTreeView |
| :has-siblings | 有兄弟目录的，例如 QTreeView |
| :adjoins-item | QTreeView 的一个 branch 存在毗邻下一个与自己不是兄弟项目的项 |
| :first | 部件的第一个，例如 QTabBar 的第一个 tab |
| :last | 最后一个，例如 :QTabBar 的最后一个 tab |
| :left | 位于左面，例如 QTabBar 的 tab 位于左面的那个 |
| :bottom | 在 item 的下面，例如 QTabBar 的 tab 按钮在下面 |
| :middle | 在列表中中间位置，例如 QTabBar 的 tab 不是最后一个也不是第一个的 |
| :minimized | 最小化的时候 |
| :maximized | 最大化状态 |
| :alternate | 当 QAstractitemView 的 QAstractitemView::alternatingRowColors() 的属性设置为 true 时，行之间背景颜色交替颜色变化 |

思维导图如下：

![](https://s2.loli.net/2022/03/22/s7IZHhVqNUEjSLF.png)

  

四、属性
----

它是一个窗口部件所固有的特征、性质，每一个窗口部件都会有属于他们自己的属性。如前面做的小例子中我们一直未曾提过 color,width,height 等。组合多个属性同时使用设计出多种效果。

### 4.1 Box样式

*   width：宽度
*   height：高度
*   max-width：最大宽度
*   max-height：最大高度
*   min-width：最小宽度
*   min-height：最小高度
*   margin：边距尺寸
    *   margin-left
    *   margin-top
    *   margin-right
    *   margin-bottom
*   padding：填充尺寸（top、right、bottom、left 顺时针顺序）
    *   padding-left
    *   padding-top
    *   padding-right
    *   padding-bottom

### 4.2 位置样式

*   position：定位属性  
    如果 position 是 relative（默认值），则将子控件移动一定的偏移量；如果 position 是 absolute，则指定子控件相对于父控件位置
*   top
*   right
*   bottom
*   left

### 4.3 字体样式

*   font：字体样式
    *   font-family：字体类型
    *   font-size：字体大小
    *   font-style：字体风格
    *   font-weight：字体粗细
    *   color：字体颜色

### 4.4文本样式

*   text-decoration：文本修饰
*   text-align：水平对齐

### 4.5 背景样式

*   background：背影样式
    
    *   background-color：背景颜色
    *   background-image：背景图片
    *   background-repeat：背景重复
    *   background-position：背景定位
    *   background-attachment：背景固定
*   background-clip：设置元素的背景（背景图片或颜色）是否延伸到边框下面。
    
*   background-origin：指定背景图片 background-image 属性的原点位置的背景相对区域
    
*   background 的简写方法
    
    ```css
    QWidget{
    	background:#000 url(..) repeat fixed left top;
    }
    ```
    

### 4.6 边框样式

*   border：边框样式
*   border-top
    *   border-top-color
    *   border-top-style
    *   border-top-width
*   border-right
    *   border-right-color
    *   border-right-style
    *   border-right-width
*   border-bottom
    *   border-bottom-color
    *   border-bottom-style
    *   border-bottom-width
*   border-left
    *   border-left-color
    *   border-left-style
    *   border-left-width
*   border-color：边框颜色
*   border-style：边框风格
*   border-width：边框宽度
*   border-image：边框图片
*   border-radius：元素的外边框圆角
    *   border-top-left-radius
    *   border-top-right-radius
    *   border-bottom-right-radius
    *   border-bottom-left-radius

### 4.7 颜色样式

*   alternate-background-color：交替行颜色
*   gridline-color：QTableView 中网格线的颜色
*   selection-color：所选文本或项目的前景色
*   selection-background-color：所选文本或项目的背景色

### 4.8 文本样式

*   outline：轮廓属性
*   outline-color：设置一个元素轮廓的颜色
*   outline-offset：设置 outline 与元素边缘或边框之间的间隙
*   outline-style：设置元素轮廓的样式
*   outline-radius：设置元素的轮廓圆角
*   outline-bottom-left-radius
*   outline-bottom-right-radius
*   outline-top-left-radius
*   outline-top-right-radius

### 4.9 其他样式

*   opacity：控件的不透明度（仅支持QToolTip）
*   icon-size：控件中图标的宽度和高度
*   image：在子控件的内容矩形中绘制的图像
*   image-position：在 Qt 4.3 及更高版本中，可以使用相对或绝对位置指定图像图像位置的对齐
*   spacing：控件中的内部间距
*   subcontrol-origin：父元素中子控件的原始矩形
*   subcontrol-position：subcontrol-origin 指定的原始矩形内子控件的对齐方式。
*   button-layout：QDialogButtonBox 或 QMessageBox 中按钮的布局
*   messagebox-text-interaction-flags：消息框中文本的交互行为
*   dialogbuttonbox-buttons-have-icons：QDialogButtonBox 中的按钮是否显示图标
*   titlebar-show-tooltips-on-buttons：是否在窗口标题栏按钮上显示工具提示
*   widget-animation-duration：动画应该持续多少（以毫秒为单位）。值等于零意味着将禁用动画
*   lineedit-password-character：该 QLineEdit 的密码字符作为 Unicode 数字
*   lineedit-password-mask-delay：在将 lineedit-password-character 应用于可见字符之前，QLineEdit 密码掩码延迟（以毫秒为单位）
*   paint-alternating-row-colors-for-empty-area：QTreeView 是否为空白区域（即没有项目的区域）绘制交替的行颜色
*   show-decoration-selected：控制 QListView 中的选择是覆盖整个行还是仅覆盖文本的范围

五、值
---

是属性`:`后面跟随的一组数字、颜色或者是一个 bool 类型等这些我们称它为值，这些值决定了窗口部件的最终的展示效果。

是属性`:`后面跟随的一组数字、颜色或者是一个 bool 类型等这些我们称它为值，这些值决定了窗口部件的最终的展示效果。

| 属性:值      | 语法                                                         | 说明                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Alignment    | { top \| bottom \| leftright \| center }                     | 方位                                                         |
| Attachment   | { scroll \| fixed }*                                         | 背景是否跟随滚动条滚动                                       |
| Background   | { Brush \| Ur1 \| Repeat \| Alignment }*                     | 任意的一个代替在查找下一层（笔刷，路径，重复，方位)。        |
| Border       | { Border Style l Length \| Brush }*                          | 简单的设置线的属性，长度，笔刷                               |
| Boolean      | 0\|1                                                         | True（ 1） false（ 0）                                       |
| Border Image | none \| Url Number{4} (stretch \| repeat)                    | 看九宫格分割法                                               |
| Border Style | dashed \| dot-dash \| dot-dot-dash \| dotted \| double \| groove \| inset \| outset \| ridge \| solid \| none | 用这些值绘制一个 border                                      |
| Box Colors   | Brush                                                        | 可以设置四个方向的颜色顺序为上， 右， 下， 左， 缺 省方式参考下面 Box Lengths。 |
| Box Lengths  | Length                                                       | 四个方向顺序为 top, right, bottom, and left 注释皆为此顺序。 例： 1.QLabel { border-width: 1px } /* 1px 1px 1px 1px */ 当输入一个值如 1px 则默认 QLable 的四个方向的值 都为 1px。 此时是 right 默认用 top 的值， bottom 自动用 top 的值， right 自动用 right 的 2.QLabel { border-width: 1px 2px } /* 1px 2px 1px 2px */ 输入两个值则表示 bottom 值和 right 的都使用默认 的值 bottom 使用 top， right 使用 left。 3.QLabel { border-width: 1px 2px 3px } /* 1px 2px 3px 2px */ 输入三个值表示最后一个 left 值默认用 right 侧的 值 4. QLabel { border-width: 1px 2px 3px 4px } /* 1px 2px 3px 4px */ |
| Brush        | Color \| Gradient                                            | 指定颜色\|渐变\|调色板                                       |
| Color        | rgb(r, g, b) \| rgba(r, g, b, a) \| hsv(h, s, v) \| hsva(h, s, v, a) \| #rrggbb \| Color Name | 颜色 rgba 和 hsva 后面的参数是透明度。                       |
| Font         | (Font Style \| Font Weight) {0,2} Font Size String           | 简单设置文字字体                                             |
| Font Size    | Length                                                       | 文字字体大小                                                 |
| Font Style   | normal \| italic \| oblique                                  | 文字字体的格式                                               |
| FontWeight   | normal \| bold \| 100 \| 200 ... \| 900                      | 文字的磅 bold 加粗                                           |
| Gradient     | qlineargradient \|qradialgradient \|qconicalgradient         | 渐变器（ 线性的渐变 \|径向渐变（ 辐射渐变） \|梯形渐变） 边界的模式是左上角（ 0,0）， 右下角（ 1,1） 参数是 从 0 到 1， 一般为实际的盒模式的坐标。 必须是升序 排序。 |
| Icon         | (Url (disabled \| active \|normal \| selected) ? ( on \| off)? )* | 一组图标地址 + 按钮的（ 禁用\|活动\|正常\|选择） ？ （ on\|off） 意思是前面括号里四个中的一个状态和后面两个中 的一个的组合来控制图片的显示哪一个。 例： * |
| Length       | Number(px \| pt \| em \| ex)?                                | 长度单位（ 数字+其中一个）                                   |
| Origin       | margin \| border \| padding \| content                       | margin: 最外边的矩形. 可以控制两个部件之间的 空隙。 border: 为边界预留的空间。 padding: 在 border 里面控制 border 与部件内容的 空间。 content: 部件窗口内容。 可以参考 The Box Model. |
| Radius       | Length                                                       | 角的弧度 1.可以是一个值这样就是角的直接弧度。 2.两个值的时候， 第一个是横向的弧度， 第二个是纵向的弧度。 可以制作出一个类似椭圆四分之一圆的角度。 |

六、逻辑否(!)
--------

有时候我们在设置某种状态的属性时，希望同时在某些非(!)的状态下设置，这个时候我们就要用(!)来选择某种状态，比如 !checked 、!has-children（没有子目录）等等。

七、盒模型(The Box Model)
--------------------

这个模式指定了 4 个影响布局的矩形，从而绘制一个自定义的窗口部件。

1.**Content** rectangle是最里面的矩形，它绘制窗口部件内容（如文字，图片）的地方。

2.**padding** rectangle包围content rectangle。它负责由padding属性指定填充操作。主要是窗口部件内容与边缘线(border)之间的空隙，它可以用top，right,bottom和 left设置它的大小。

3.**border** rectangle包围padding rectangle。它为边界预留空间。可以认为是窗口的外框线。下面讲的分割图形的方法中把 border 当做是一个区域来理解的。参考四、高级应用:九宫格分割法

4.**margin** rectangle最外面的矩形，他包围border rectangle，负责指定的边缘空白区域，主要是负责与其他的窗口部件的距离。

如果没有指定他们四个，则默认是四个重合在一起的。如下图：

![](https://s2.loli.net/2022/02/24/TMY5Wi8IhSbrXtO.png)

  

八、背景色和前景色
---------

部件的前景色用于绘制窗口部件上面的文本， 可以通过 color 属性指定。

背景色用于绘制窗口部件的填充矩形， 可以通过 background-color 属性指定。

背景图片使用 background-image 属性定义， 它用于绘制由 background-origin 指定在盒模式中四个区域的图片开始显示的起点位置。 背景图片在盒模式域内的对齐和平铺方式可以通过 background-position 和 background-repeat 属性指定。

如果指定的背景图片具有 alpha 通道（ 透明效果），通过 background-color 指定的颜色将会透过透明区域。在 background-color 属性中有介绍。



> [Qt Style Sheets Reference](https://doc.qt.io/qt-6/stylesheet-reference.html)