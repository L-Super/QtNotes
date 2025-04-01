用于播放简单的动画（GIF 等）。

首先，通过向 QMovie 的构造函数传递文件名或指向包含动画图像格式的 QIODevice 的指针，创建一个 QMovie 对象。在启动影片前，您可以调用 `isValid()` 来检查图像数据是否有效。要启动影片，请调用 `start()` 。QMovie 将进入 Running 状态，并发出 `started()` 和 `stateChanged()` 信号。要获取影片的当前状态，请调用 `state()` 。

示例：
```cpp
QLabel label;
QMovie *movie = new QMovie("animations/fire.gif");

label.setMovie(movie);
movie->start();
```

每当影片中有新的帧可用时，QMovie 就会发出 `updated()` 。如果帧的大小发生变化，`resized()` 将被发送。可以调用 `currentImage()` 或 `currentPixmap()` 来获取当前帧的副本。影片播放完毕后，QMovie 会发送 `finished()` 。如果播放过程中出现任何错误（如图像文件损坏），QMovie 将发出 `error()` 。

您可以通过调用 `setSpeed()` 来控制影片的播放速度，它将原始速度的百分比作为参数。调用 `setPaused(true)` 可暂停影片播放。然后，QMovie 将进入 Paused 状态并发出 `stateChanged()` 。如果调用 `setPaused(false)`，QMovie 将重新进入 Running 状态并再次启动影片。要停止影片，请调用 `stop()` 。

某些动画格式允许您设置背景颜色。可以调用 `setBackgroundColor()` 来设置颜色，或调用 `backgroundColor()` 来获取当前的背景颜色。

`currentFrameNumber()` 返回当前帧的序列号。如果图像格式支持序列号，`frameCount()` 会返回动画的总帧数。可以调用 `loopCount()` 来获取动画在结束前的循环次数。`nextFrameDelay()` 返回当前帧应显示的毫秒数。

调用 `setCacheMode()` 可指示 QMovie 缓存动画帧。

调用 `supportedFormats()` 查看 QMovie 支持的格式列表。

## 公共功能

|                                 |                                                                                                          |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- |
|                                 | **QMovie**(QObject *parent = nullptr)                                                                    |
|                                 | **QMovie**(QIODevice *device, const QByteArray &format = QByteArray (), QObject *parent = nullptr)       |
|                                 | **QMovie**(const QString &fileName, const QByteArray &format = QByteArray (), QObject *parent = nullptr) |
| virtual                         | **~QMovie**()                                                                                            |
| QColor                          | **backgroundColor**() const                                                                              |
| QBindable<QMovie::CacheMode>    | **bindableCacheMode**()                                                                                  |
| QBindable<int>                  | **bindableSpeed**()                                                                                      |
| QMovie:: CacheMode              | **cacheMode**() const                                                                                    |
| int                             | **currentFrameNumber**() const                                                                           |
| QImage                          | **currentImage**() const                                                                                 |
| QPixmap                         | **currentPixmap**() const                                                                                |
| QIODevice *                     | **device**() const                                                                                       |
| QString                         | **fileName**() const                                                                                     |
| QByteArray                      | **format**() const                                                                                       |
| int                             | **frameCount**() const                                                                                   |
| QRect                           | **frameRect**() const                                                                                    |
| bool                            | **isValid**() const                                                                                      |
| bool                            | **jumpToFrame**(int frameNumber)                                                                         |
| QImageReader:: ImageReaderError | **lastError**() const                                                                                    |
| QString                         | **lastErrorString**() const                                                                              |
| int                             | **loopCount**() const                                                                                    |
| int                             | **nextFrameDelay**() const                                                                               |
| QSize                           | **scaledSize**()                                                                                         |
| void                            | **setBackgroundColor**(const QColor &color)                                                              |
| void                            | **setCacheMode**(QMovie:: CacheMode mode)                                                                |
| void                            | **setDevice**(QIODevice *device)                                                                         |
| void                            | **setFileName**(const QString &fileName)                                                                 |
| void                            | **setFormat**(const QByteArray &format)                                                                  |
| void                            | **setScaledSize**(const QSize &size)                                                                     |
| int                             | **speed**() const                                                                                        |
| QMovie:: MovieState             | **state**() const                                                                                        |

