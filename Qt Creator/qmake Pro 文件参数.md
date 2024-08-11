| Pro变量参数        | 含义                                               | 示例                                      |
| -------------- | -------------------------------------------------- | ----------------------------------------- |
| TEMPLATE       | 模板变量指定生成makefile(app:应用程序/lib:库)      | TEMPLATE = app                            |
| QT             | 使用到的Qt定义的类(core/gui/widgets...)            | QT += widgtes                             |
| DESTDIR        | 指定生成的应用程序放置的目录                       | DESTDIR += ../bin                         |
| TARGET         | 指定生成的应用程序名                               | TARGET = hello                            |
| HEADERS        | 工程中包含的头文件                                 | HEADERS += hello.h                        |
| FORMS          | 工程中包含的.ui设计文件                            | FORMS += hello.ui                         |
| SOURCES        | 工程中包含的源文件                                 | SOURCES += main.cpp hello.cpp             |
| RESOURCES      | 工程中包含的资源文件                               | RESOURCES += qrc/hello.qrc                |
| LIBS           | 引入的lib文件的路径 -L：引入路径                   | LIBS += -L.                               |
| CONFIG         | 用来告诉qmake关于应用程序的配置信息                | CONFIG+= qt warn_on release               |
| UI_DIR         | 指定.ui文件转化成`ui_*.h`文件的存放目录            | UI_DIR += forms                           |
| RCC_DIR        | 指定将.qrc文件转换成`qrc_*.h`文件的存放目录        | RCC_DIR += ../tmp                         |
| MOC_DIR        | 指定将含Q_OBJECT的头文件转换成标准.h文件的存放目录 | MOC_DIR += ../tmp                         |
| OBJECTS_DIR    | 指定目标文件(obj)的存放目录                        | OBJECTS_DIR += ../tmp                     |
| DEPENDPATH     | 程序编译时依赖的相关路径                           | DEPENDPATH += . forms include qrc sources |
| INCLUDEPATH    | 头文件包含路径                                     | INCLUDEPATH += .                          |
| DEFINES        | 增加预处理器宏（gcc的-D选项）。                    | DEFINES += USE_MY_STUFF                   |
| QMAKE_CFLAGS   | 设置c编译器flag参数                                | QMAKE_CFLAGS += -g                        |
| QMAKE_CXXFLAGS | 设置c++编译器flag参数                              | QMAKE_CXXFLAGS += -g                      |
| QMAKE_LFLAGS   | 设置链接器flag参数                                 | QMAKE_LFLAGS += -rdynamic                 |