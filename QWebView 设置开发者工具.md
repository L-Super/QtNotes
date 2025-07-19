```cpp
auto webView = new QWebEngineView(this);

auto shortcut = new QShortcut(QKeySequence("F12"), this);

devTools = new QWebEngineView();

connect(shortcut, &QShortcut::activated, this, [this, page = webView->page()]() {
	page->setDevToolsPage(devTools->page());
	page->triggerAction(QWebEnginePage::InspectElement);
	devTools->show();
});
```