VS2019的C++语言标准默认是ISO C++14，可是通过VS新建的Qt工程，默认是C++11，那么如何更改为支持C++14呢


可以查看正在使用中的C++标准

```C++
cout << __cplusplus << endl;
```

如果输出为`199711`，那么就说明使用的是C++11语言标准

为什么VS2019项目属性里C++语言标准默认是ISO C++14，但实际的工程还是C++11呢?原因应该是这个
> **_ _ cplusplus** 预处理器宏通常用于报告对特定版本的 C++ 标准的支持。 因为很多现有代码需要此宏的值与“199711L”匹配，所以编译器不会更改此宏的值，除非通过使用 /Zc:__cplusplus 编译器选项进行显式选择。

[/Zc:__cplusplus（启用更新的 __cplusplus 宏）](https://docs.microsoft.com/zh-cn/cpp/build/reference/zc-cplusplus?view=msvc-160&viewFallbackFrom=vs-2019)

进行如下设置即可更新宏，【右击项目】–【选择属性】–【C/C++】–【命令行】–【其他选项】，写入如下内容：

```C++
/Zc:__cplusplus 
```

再次运行代码，查看输出内容，此时就变为`201402`，说明已经成功更新为C++14
