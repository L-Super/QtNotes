
QMenuBar 作为载体（菜单 QMenu 挂载在菜单栏 QMenuBar 上，即 QMenuBar 只是作为容器，并不作为控件表示出现）
```cpp
menu = new QMenu(this);  
menu->setIcon(QIcon(":resources/images/menu.svg"));  
companySettingAction = new QAction(menu);  
companySettingAction->setText("企业设置");  
menu->addAction(companySettingAction);  
versionUpdateAction = new QAction(menu);  
versionUpdateAction->setText("版本检查");  
menu->addAction(versionUpdateAction);  
companySettingAction->setIcon(QIcon(":resources/images/no-red-dot.svg"));  
versionUpdateAction->setIcon(QIcon(":resources/images/no-red-dot.svg"));  
  
menuBar.addMenu(menu);  
  
connect(companySettingAction, &QAction::triggered, this, &WindowControlButtons::ShowCompanySettingDialog);  
connect(versionUpdateAction, &QAction::triggered, this, &WindowControlButtons::ShowUpdateDialog);  
connect(versionUpdateAction, &QAction::triggered, this, &WindowControlButtons::SetMenuIconWithRedDot);
```

也可将 QPushButton 作为载体。若将菜单加入按钮，那么按钮会有下拉的三角形图标，也可用 QSS 去除。
```cpp
menuPushButton.setMenu(menu);  
// 取消按钮显示下拉的三角形图标  
menuPushButton.setStyleSheet("QPushButton::menu-indicator{"  
"image:none;}");  
```
