可以自定义 qDebug 打印信息的宏
```c++
  #define cout qDebug()<<"["<<__FILE__<<__func__<<__LINE__<<"]"
```
输出将是
`[ ..\QChat\ChatWindow. cpp on_sendBtn_clicked 37 ] send button clicked!`

`__FILE__` ：当前源文件路径及文件名；  
`__LINE__` ：当前源代码行号；
`__func__` : 当前的函数名；（ `__FUNCTION__` 也可以实现）