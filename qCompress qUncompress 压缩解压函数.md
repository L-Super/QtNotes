`qCompress` 与 `qUncompress` 为QByteArray相关非成员函数
```cpp
QByteArray qCompress(const QByteArray &data, int compressionLevel = -1)
QByteArray qCompress(const uchar *data, qsizetype nbytes, int compressionLevel = -1)

QByteArray qUncompress(const QByteArray &data)
QByteArray qUncompress(const uchar *data, qsizetype nbytes)
```

## qCompress
压缩数据字节数组并返回新字节数组中的压缩数据。

`compressionLevel` 参数指定使用的压缩程度。有效值在0和9之间，其中9对应于以使用较慢算法为代价的最大压缩（即较小的压缩数据）。较小的值（8，7，…，1）在稍快的速度下连续提供较小的压缩。值0对应于根本没有压缩。默认值是-1，它指定zlib的默认压缩。

## qUncompress
解压数据字节数组并返回一个包含未压缩数据的新字节数组。

如果输入数据已损坏，则返回空 QByteArray。

这个函数将把用 `qCompress()` 压缩的数据解压缩。

>[!note] 
>如果想使用这个函数来解压使用 zlib 压缩的外部数据，您首先需要在包含数据的字节数组前面添加一个四字节头。报头必须包含未压缩数据的预期长度 (以字节为单位) ，以无符号、大端序、32位整数表示。但是，这个数字只是输出缓冲区大小的初始大小的一个提示。如果指定的大小太小而不能容纳结果，则输出缓冲区大小仍将增加，直到输出匹配或系统耗尽内存为止。因此，尽管有32位的头部，这个函数在64位平台上可以产生超过4GB 的输出。
>

另外：
>[!note] 
>在 Qt 6.5之前的 Qt 版本中，超过2GB 的数据工作不可靠; 在 Qt 6.0之前的 Qt 版本中，根本不可靠。
