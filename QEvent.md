QEvent 类是所有事件类的基类，事件对象包含事件参数。



## 枚举 QEvent::Type

这个枚举类型定义了 Qt 中有效的事件类型。事件类型和每个类型的专门类如下：

| 常量                                       | 值                   | 描述                                                            |
| ---------------------------------------- | ------------------- | ------------------------------------------------------------- |
| QEvent::None                             | 0                   | 不是一个事件                                                        |
| QEvent::ActionAdded                      | 114                 | 一个新 action 被添加（QActionEvent）                                  |
| QEvent::ActionChanged                    | 113                 | 一个 action 被改变（QActionEvent）                                   |
| QEvent::ActionRemoved                    | 115                 | 一个 action 被移除（QActionEvent）                                   |
| QEvent::ActivationChange                 | 99                  | Widget 的顶层窗口激活状态发生了变化                                         |
| QEvent::ApplicationActivate              | 121                 | 这个枚举已被弃用，使用 ApplicationStateChange 代替                         |
| QEvent::ApplicationActivated             | ApplicationActivate | 这个枚举已被弃用，使用 ApplicationStateChange 代替                         |
| QEvent::ApplicationDeactivate            | 122                 | 这个枚举已被弃用，使用 ApplicationStateChange 代替                         |
| QEvent::ApplicationFontChange            | 36                  | 应用程序的默认字体发生了变化                                                |
| QEvent::ApplicationLayoutDirectionChange | 37                  | 应用程序的默认布局方向发生了变化                                              |
| QEvent::ApplicationPaletteChange         | 38                  | 应用程序的默认调色板发生了变化                                               |
| QEvent::ApplicationStateChange           | 214                 | 应用程序的状态发生了变化                                                  |
| QEvent::ApplicationWindowIconChange      | 35                  | 应用程序的图标发生了变化                                                  |
| QEvent::ChildAdded                       | 68                  | 一个对象获得孩子（QChildEvent）                                         |
| QEvent::ChildPolished                    | 69                  | 一个部件的孩子被抛光（QChildEvent）                                       |
| QEvent::ChildRemoved                     | 71                  | 一个对象时区孩子（QChildEvent）                                         |
| QEvent::Clipboard                        | 40                  | 剪贴板的内容发生改变                                                    |
| QEvent::Close                            | 19                  | Widget 被关闭（QCloseEvent）                                       |
| QEvent::CloseSoftwareInputPanel          | 200                 | 一个部件要关闭软件输入面板（SIP）                                            |
| QEvent::ContentsRectChange               | 178                 | 部件内容区域的外边距发生改变                                                |
| QEvent::ContextMenu                      | 82                  | 上下文弹出菜单（QContextMenuEvent）                                    |
| QEvent::CursorChange                     | 183                 | 部件的鼠标发生改变                                                     |
| QEvent::DeferredDelete                   | 52                  | 对象被清除后将被删除（QDeferredDeleteEvent）                              |
| QEvent::DragEnter                        | 60                  | 在拖放操作期间鼠标进入窗口部件（QDragEnterEvent）                              |
| QEvent::DragLeave                        | 62                  | 在拖放操作期间鼠标离开窗口部件（QDragLeaveEvent）                              |
| QEvent::DragMove                         | 61                  | 拖放操作正在进行（QDragMoveEvent）                                      |
| QEvent::Drop                             | 63                  | 拖放操作完成（QDropEvent）                                            |
| QEvent::DynamicPropertyChange            | 170                 | 动态属性已添加、更改或从对象中删除                                             |
| QEvent::EnabledChange                    | 98                  | 部件的 enabled 状态已更改                                             |
| QEvent::Enter                            | 10                  | 鼠标进入部件的边界（QEnterEvent）                                        |
| QEvent::EnterEditFocus                   | 150                 | 编辑部件获得焦点进行编辑，必须定义 QT_KEYPAD_NAVIGATION                        |
| QEvent::EnterWhatsThisMode               | 124                 | 当应用程序进入 “What’s This?” 模式，发送到 toplevel 顶层部件                   |
| QEvent::Expose                           | 206                 | 当其屏幕上的内容无效，发送到窗口，并需要从后台存储刷新                                   |
| QEvent::FileOpen                         | 116                 | 文件打开请求（QFileOpenEvent）                                        |
| QEvent::FocusIn                          | 8                   | 部件或窗口获得键盘焦点（QFocusEvent）                                      |
| QEvent::FocusOut                         | 9                   | 部件或窗口失去键盘焦点（QFocusEvent）                                      |
| QEvent::FocusAboutToChange               | 23                  | 部件或窗口焦点即将改变（QFocusEvent）                                      |
| QEvent::FontChange                       | 97                  | 部件的字体发生改变                                                     |
| QEvent::Gesture                          | 198                 | 触发了一个手势（QGestureEvent）                                        |
| QEvent::GestureOverride                  | 202                 | 触发了手势覆盖（QGestureEvent）                                        |
| QEvent::GrabKeyboard                     | 188                 | Item 获得键盘抓取（仅限 QGraphicsItem）                                 |
| QEvent::GrabMouse                        | 186                 | 项目获得鼠标抓取（仅限 QGraphicsItem）                                    |
| QEvent::GraphicsSceneContextMenu         | 159                 | 在图形场景上的上下文弹出菜单（QGraphicsScene ContextMenuEvent）               |
| QEvent::GraphicsSceneDragEnter           | 164                 | 在拖放操作期间，鼠标进入图形场景（QGraphicsSceneDragDropEvent）                 |
| QEvent::GraphicsSceneDragLeave           | 166                 | 在拖放操作期间鼠标离开图形场景（QGraphicsSceneDragDropEvent）                  |
| QEvent::GraphicsSceneDragMove            | 165                 | 在场景上正在进行拖放操作（QGraphicsSceneDragDropEvent）                     |
| QEvent::GraphicsSceneDrop                | 167                 | 在场景上完成拖放操作（QGraphicsSceneDragDropEvent）                       |
| QEvent::GraphicsSceneHelp                | 163                 | 用户请求图形场景的帮助（QHelpEvent）                                       |
| QEvent::GraphicsSceneHoverEnter          | 160                 | 鼠标进入图形场景中的悬停项（QGraphicsSceneHoverEvent）                       |
| QEvent::GraphicsSceneHoverLeave          | 162                 | 鼠标离开图形场景中一个悬停项（QGraphicsSceneHoverEvent）                      |
| QEvent::GraphicsSceneHoverMove           | 161                 | 鼠标在图形场景中的悬停项内移动（QGraphicsSceneHoverEvent）                     |
| QEvent::GraphicsSceneMouseDoubleClick    | 158                 | 鼠标在图形场景中再次按下（双击）（QGraphicsSceneMouseEvent）                    |
| QEvent::GraphicsSceneMouseMove           | 155                 | 鼠标在图形场景中移动（QGraphicsSceneMouseEvent）                          |
| QEvent::GraphicsSceneMousePress          | 156                 | 鼠标在图形场景中按下（QGraphicsSceneMouseEvent）                          |
| QEvent::GraphicsSceneMouseRelease        | 157                 | 鼠标在图形场景中释放（QGraphicsSceneMouseEvent）                          |
| QEvent::GraphicsSceneMove                | 182                 | 部件被移动（QGraphicsSceneMoveEvent）                                |
| QEvent::GraphicsSceneResize              | 181                 | 部件已调整大小（QGraphicsSceneResizeEvent）                            |
| QEvent::GraphicsSceneWheel               | 168                 | 鼠标滚轮在图形场景中滚动（QGraphicsSceneWheelEvent）                        |
| QEvent::Hide                             | 18                  | 部件被隐藏（QHideEvent）                                             |
| QEvent::HideToParent                     | 27                  | 子部件被隐藏（QHideEvent）                                            |
| QEvent::HoverEnter                       | 127                 | 鼠标进入悬停部件（QHoverEvent）                                         |
| QEvent::HoverLeave                       | 128                 | 鼠标留离开悬停部件（QHoverEvent）                                        |
| QEvent::HoverMove                        | 129                 | 鼠标在悬停部件内移动（QHoverEvent）                                       |
| QEvent::IconDrag                         | 96                  | 窗口的主图标被拖走（QIconDragEvent）                                     |
| QEvent::IconTextChange                   | 101                 | 部件的图标文本发生改变（已弃用）                                              |
| QEvent::InputMethod                      | 83                  | 正在使用输入法（QInputMethodEvent）                                    |
| QEvent::InputMethodQuery                 | 207                 | 输入法查询事件（QInputMethodQueryEvent）                               |
| QEvent::KeyboardLayoutChange             | 169                 | 键盘布局已更改                                                       |
| QEvent::KeyPress                         | 6                   | 键盘按下（QKeyEvent）                                               |
| QEvent::KeyRelease                       | 7                   | 键盘释放（QKeyEvent）                                               |
| QEvent::LanguageChange                   | 89                  | 应用程序翻译发生改变                                                    |
| QEvent::LayoutDirectionChange            | 90                  | 布局的方向发生改变                                                     |
| QEvent::LayoutRequest                    | 76                  | 部件的布局需要重做                                                     |
| QEvent::Leave                            | 11                  | 鼠标离开部件的边界                                                     |
| QEvent::LeaveEditFocus                   | 151                 | 编辑部件失去编辑的焦点，必须定义 QT_KEYPAD_NAVIGATION                         |
| QEvent::LeaveWhatsThisMode               | 125                 | 当应用程序离开 “What’s This?” 模式，发送到顶层部件                             |
| QEvent::LocaleChange                     | 88                  | 系统区域设置发生改变                                                    |
| QEvent::NonClientAreaMouseButtonDblClick | 176                 | 鼠标双击发生在客户端区域外                                                 |
| QEvent::NonClientAreaMouseButtonPress    | 174                 | 鼠标按钮按下发生在客户端区域外                                               |
| QEvent::NonClientAreaMouseButtonRelease  | 175                 | 鼠标按钮释放发生在客户端区域外                                               |
| QEvent::NonClientAreaMouseMove           | 173                 | 鼠标移动发生在客户区域外                                                  |
| QEvent::MacSizeChange                    | 177                 | 用户更改了部件的大小（仅限 OS X）                                           |
| QEvent::MetaCall                         | 43                  | 通过 QMetaObject::invokeMethod() 调用异步方法                         |
| QEvent::ModifiedChange                   | 102                 | 部件修改状态发生改变                                                    |
| QEvent::MouseButtonDblClick              | 4                   | 鼠标再次按下（QMouseEvent）                                           |
| QEvent::MouseButtonPress                 | 2                   | 鼠标按下（QMouseEvent）                                             |
| QEvent::MouseButtonRelease               | 3                   | 鼠标释放（QMouseEvent）                                             |
| QEvent::MouseMove                        | 5                   | 鼠标移动（QMouseEvent）                                             |
| QEvent::MouseTrackingChange              | 109                 | 鼠标跟踪状态发生改变                                                    |
| QEvent::Move                             | 13                  | 部件的位置发生改变（QMoveEvent）                                         |
| QEvent::NativeGesture                    | 197                 | 系统检测到手势（QNativeGestureEvent）                                  |
| QEvent::OrientationChange                | 208                 | 屏幕方向发生改变（QScreenOrientationChangeEvent）                       |
| QEvent::Paint                            | 12                  | 需要屏幕更新（QPaintEvent）                                           |
| QEvent::PaletteChange                    | 39                  | 部件的调色板发生改变                                                    |
| QEvent::ParentAboutToChange              | 131                 | 部件的 parent 将要更改                                               |
| QEvent::ParentChange                     | 21                  | 部件的 parent 发生改变                                               |
| QEvent::PlatformPanel                    | 212                 | 请求一个特定于平台的面板                                                  |
| QEvent::PlatformSurface                  | 217                 | 原生平台表面已创建或即将被销毁（QPlatformSurfaceEvent）                        |
| QEvent::Polish                           | 75                  | 部件被抛光                                                         |
| QEvent::PolishRequest                    | 74                  | 部件应该被抛光                                                       |
| QEvent::QueryWhatsThis                   | 123                 | 如果部件有 “What’s This?” 帮助，应该接受事件                                |
| QEvent::ReadOnlyChange                   | 106                 | 部件的 read-only 状态发生改变                                          |
| QEvent::RequestSoftwareInputPanel        | 199                 | 部件想要打开软件输入面板（SIP）                                             |
| QEvent::Resize                           | 14                  | 部件的大小发生改变（QResizeEvent）                                       |
| QEvent::ScrollPrepare                    | 204                 | 对象需要填充它的几何信息（QScrollPrepareEvent）                             |
| QEvent::Scroll                           | 205                 | 对象需要滚动到提供的位置（QScrollEvent）                                    |
| QEvent::Shortcut                         | 117                 | 快捷键处理（QShortcutEvent）                                         |
| QEvent::ShortcutOverride                 | 51                  | 按下按键，用于覆盖快捷键（QKeyEvent）                                       |
| QEvent::Show                             | 17                  | 部件显示在屏幕上（QShowEvent）                                          |
| QEvent::ShowToParent                     | 26                  | 子部件被显示                                                        |
| QEvent::SockAct                          | 50                  | Socket 激活，用于实现 QSocketNotifier                                |
| QEvent::StateMachineSignal               | 192                 | 信号被传递到状态机（QStateMachine::SignalEvent）                         |
| QEvent::StateMachineWrapped              | 193                 | 事件是一个包装器，用于包含另一个事件（QStateMachine::WrappedEvent）               |
| QEvent::StatusTip                        | 112                 | 状态提示请求（QStatusTipEvent）                                       |
| QEvent::StyleChange                      | 100                 | 部件的样式发生改变                                                     |
| QEvent::TabletMove                       | 87                  | Wacom 写字板移动（QTabletEvent）                                     |
| QEvent::TabletPress                      | 92                  | Wacom 写字板按下（QTabletEvent）                                     |
| QEvent::TabletRelease                    | 93                  | Wacom 写字板释放（QTabletEvent）                                     |
| QEvent::OkRequest                        | 94                  | Ok 按钮在装饰前被按下，仅支持 Windows CE                                   |
| QEvent::TabletEnterProximity             | 171                 | Wacom 写字板进入接近事件（QTabletEvent），发送到 QApplication                |
| QEvent::TabletLeaveProximity             | 172                 | Wacom 写字板离开接近事件（QTabletEvent），发送到 QApplication                |
| QEvent::ThreadChange                     | 22                  | 对象被移动到另一个线程。这是发送到此对象的最后一个事件在上一个线程中，参见：QObject::moveToThread() |
| QEvent::Timer                            | 1                   | 定时器事件（QTimerEvent）                                            |
| QEvent::ToolBarChange                    | 120                 | 工具栏按钮在 OS X 上进行切换                                             |
| QEvent::ToolTip                          | 110                 | 一个 tooltip 请求（QHelpEvent）                                     |
| QEvent::ToolTipChange                    | 184                 | 部件的 tooltip 发生改变                                              |
| QEvent::TouchBegin                       | 194                 | 触摸屏或轨迹板事件序列的开始（QTouchEvent）                                   |
| QEvent::TouchCancel                      | 209                 | 取消触摸事件序列（QTouchEvent）                                         |
| QEvent::TouchEnd                         | 196                 | 触摸事件序列结束（QTouchEvent）                                         |
| QEvent::TouchUpdate                      | 195                 | 触摸屏事件（QTouchEvent）                                            |
| QEvent::UngrabKeyboard                   | 189                 | Item 失去键盘抓取（QGraphicsItem）                                    |
| QEvent::UngrabMouse                      | 187                 | Item 失去鼠标抓取（QGraphicsItem、QQuickItem）                         |
| QEvent::UpdateLater                      | 78                  | 部件应该排队在以后重新绘制                                                 |
| QEvent::UpdateRequest                    | 77                  | 部件应该被重绘                                                       |
| QEvent::WhatsThis                        | 111                 | 部件应该显示 “What’s This” 帮助（QHelpEvent）                           |
| QEvent::WhatsThisClicked                 | 118                 | 部件的 “What’s This” 帮助链接被点击                                     |
| QEvent::Wheel                            | 31                  | 鼠标滚轮滚动（QWheelEvent）                                           |
| QEvent::WinEventAct                      | 132                 | 发生了 Windows 特定的激活事件                                           |
| QEvent::WindowActivate                   | 24                  | 窗口已激活                                                         |
| QEvent::WindowBlocked                    | 103                 | 窗口被模态对话框阻塞                                                    |
| QEvent::WindowDeactivate                 | 25                  | 窗户被停用                                                         |
| QEvent::WindowIconChange                 | 34                  | 窗口的图标发生改变                                                     |
| QEvent::WindowStateChange                | 105                 | 窗口的状态（最小化、最大化或全屏）发生改变（QWindowStateChangeEvent）                |
| QEvent::WindowTitleChange                | 33                  | 窗口的标题发生改变                                                     |
| QEvent::WindowUnblocked                  | 104                 | 一个模态对话框退出后，窗口将不被阻塞                                            |
| QEvent::WinIdChange                      | 203                 | 本地窗口的系统标识符发生改变                                                |
| QEvent::ZOrderChange                     | 126                 | 部件的 z 值发生了改变，该事件不会发送给顶层窗口                                     |