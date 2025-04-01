用于播放简单的动画（GIF 等）。

首先，通过向 QMovie 的构造函数传递文件名或指向包含动画图像格式的 QIODevice 的指针，创建一个 QMovie 对象。在启动影片前，您可以调用 `isValid()` 来检查图像数据是否有效。要启动影片，请调用 `start()` 。QMovie 将进入 Running 状态，并发出 `started()` 和 `stateChanged()` 信号。要获取影片的当前状态，请调用 `state()` 。

示例：
```cpp
QLabel label;
QMovie *movie = new QMovie("animations/fire.gif");

label.setMovie(movie);
movie->start();
```
这种方式通常会比较模糊，最佳实践是取出当前帧的图片，使用 `paintEvent()` 绘制。


每当影片中有新的帧可用时，QMovie 就会发出 `updated()` 。如果帧的大小发生变化，`resized()` 将被发送。可以调用 `currentImage()` 或 `currentPixmap()` 来获取当前帧的副本。影片播放完毕后，QMovie 会发送 `finished()` 。如果播放过程中出现任何错误（如图像文件损坏），QMovie 将发出 `error()` 。

您可以通过调用 `setSpeed()` 来控制影片的播放速度，它将原始速度的百分比作为参数。调用 `setPaused(true)` 可暂停影片播放。然后，QMovie 将进入 Paused 状态并发出 `stateChanged()` 。如果调用 `setPaused(false)`，QMovie 将重新进入 Running 状态并再次启动影片。要停止影片，请调用 `stop()` 。

某些动画格式允许您设置背景颜色。可以调用 `setBackgroundColor()` 来设置颜色，或调用 `backgroundColor()` 来获取当前的背景颜色。

`currentFrameNumber()` 返回当前帧的序列号。如果图像格式支持序列号，`frameCount()` 会返回动画的总帧数。可以调用 `loopCount()` 来获取动画在结束前的循环次数。`nextFrameDelay()` 返回当前帧应显示的毫秒数。

调用 `setCacheMode()` 可指示 QMovie 缓存动画帧。

调用 `supportedFormats()` 查看 QMovie 支持的格式列表。

## 函数

| 函数                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| QMovie(QObject *parent = nullptr)                            |                                                              |
| QMovie(QIODevice *device, const QByteArray &format = QByteArray (), QObject *parent = nullptr) |                                                              |
| QMovie(const QString &fileName, const QByteArray &format = QByteArray (), QObject *parent = nullptr) |                                                              |
| int currentFrameNumber() const                               | 返回当前帧的序列号。影片第一帧的序列号为 0。                 |
| QImage currentImage() const                                  | 以QImage 的形式返回当前帧。                                  |
| QPixmap currentPixmap() const                                | 以QPixmap 的形式返回当前帧。                                 |
| int frameCount() const                                       | 返回动画的帧数。<br/>某些动画格式不支持此功能，在这种情况下将返回 0。 |
| QRect frameRect() const                                      | 返回最后一帧的矩形。如果尚未更新任何帧，则返回无效的QRect 。 |
| bool isValid() const                                         | 如果影片有效（如图像数据可读且图像格式受支持），则返回`true` ；否则返回`false` 。 |
| bool jumpToFrame(int frameNumber)                            | 跳转到帧号*frameNumber* 。成功时返回`true` ，否则返回`false` 。 |
| QImageReader::ImageReaderError lastError() const             | 返回尝试读取图像数据时发生的最新错误。                       |
| QString lastErrorString() const                              | 返回尝试读取图像数据时发生的最新错误的字符串。               |
| int loopCount() const                                        | 返回电影结束前的循环次数。如果影片只播放一次（不循环），loopCount 返回 0；如果影片永远循环播放，loopCount 返回-1。 |
| int nextFrameDelay() const                                   | 返回QMovie 更新动画下一帧前等待的毫秒数。                    |
| QSize scaledSize()                                           | 返回按比例缩放的帧大小。                                     |
| void setBackgroundColor(const QColor &color)                 | 对于支持背景色的图像格式，该函数将背景色设置为*color* 。     |
| void setDevice(QIODevice *device)                            | 将当前设备设置为device 。当电影运行时，QMovie 将从该设备读取图像数据。 |
| void setFileName(const QString &fileName)                    | 将QMovie 读取图像数据的文件名设置为fileName 。               |
| void setFormat(const QByteArray &format)                     | 将QMovie 在解码图像数据时使用的格式设置为format 。默认情况下，QMovie 会尝试猜测图像数据的格式。 |
| void setScaledSize(const QSize &size)                        | 将缩放后的帧大小设置为*size* 。                              |



槽函数：

| 函数                            | 描述                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| bool jumpToNextFrame()          | 跳转到下一帧。成功时返回`true` ，否则返回`false` 。          |
| void setPaused(bool paused)     | 如果paused 为 true，QMovie 将进入Paused 状态并发出stateChanged （暂停）；否则将进入Running 状态并发出stateChanged （运行）。 |
| Void setSpeed(int percentSpeed) | 速度以原始影片速度的百分比来衡量。默认速度为 100%。          |
| void start()                    | 开始播放电影。将进入Running 状态，并随着电影的播放开始发出updated() 和resized() 信号。<br/><br/>如果QMovie 处于Paused 状态，则此函数等同于调用setPaused(false)。如果QMovie 已处于Running 状态，则此函数不会执行任何操作。 |
| void stop()                     | 进入NotRunning 状态，并停止发送updated() 和resized() 。如果再次调用start() ，影片将从头开始。<br/><br/>如果QMovie 已处于NotRunning 状态，则此函数不会执行任何操作。 |

信号：

| 函数                            | 描述          |
| --------------------------------------------------- | ---- |
| void error(QImageReader::ImageReaderError error) | 当播放过程中出现错误error 时，QMovie 会发出该信号。QMovie 会停止播放影片，并进入QMovie::NotRunning 状态。 |
| void finished()                                  | 该信号在电影结束时发出。 |
| void frameChanged(int frameNumber)               | 当帧编号变为frameNumber 时，就会发出该信号。您可以调用currentImage() 或currentPixmap() 来获取帧的副本。 |
| void resized(const QSize &size)                  | 当当前帧的大小调整为size 时，将发出该信号。这种效果有时会在动画中使用，作为替换帧的替代方法。您可以调用currentImage() 或currentPixmap() 来获取更新帧的副本。 |
| void started()                                   | 该信号在QMovie::start() 被调用且QMovie 已进入QMovie::Running 状态后发出。 |
| void stateChanged(QMovie::MovieState state)      | 每次影片状态发生变化时都会发出该信号。新状态由*state* 指定。 |
| void updated(const QRect &rect)                  | 当当前帧中的矩形rect 已更新时，就会发出该信号。您可以调用currentImage() 或currentPixmap() 来获取更新帧的副本。 |

