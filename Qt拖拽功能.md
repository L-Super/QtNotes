示例：
从本地文件管理器拖拽图片文件到窗口内，并显示图片
```cpp
#include <QApplication>
#include <QWidget>
#include <QLabel>
#include <QDragEnterEvent>
#include <QMimeData>
#include <QFile>
#include <QImage>
#include <QVBoxLayout>

class ImageWidget : public QWidget {
    Q_OBJECT
public:
    ImageWidget(QWidget *parent = nullptr) : QWidget(parent) {
        // 创建一个QLabel来显示图片
        imageLabel = new QLabel(this);
        imageLabel->setAlignment(Qt::AlignCenter);
        imageLabel->setMinimumSize(100, 100); // 设置最小尺寸，确保图片可见

        // 设置布局
        QVBoxLayout *layout = new QVBoxLayout(this);
        layout->addWidget(imageLabel);

        // 为窗口设置拖拽进入事件的处理函数
        setAcceptDrops(true);
    }

protected:
    void dragEnterEvent(QDragEnterEvent *event) override {
        if (event->mimeData()->hasUrls()) {
            event->acceptProposedAction();
        }
    }

    void dropEvent(QDropEvent *event) override {
        // 获取拖拽的文件URL
        QList<QUrl> urls = event->mimeData()->urls();
        if (!urls.isEmpty()) {
            QString filePath = urls.first().toLocalFile();
            QImage image(filePath);
            if (!image.isNull()) {
                // 如果图片加载成功，显示图片
                imageLabel->setPixmap(QPixmap::fromImage(image));
            } else {
                // 如果图片加载失败，显示错误信息
                imageLabel->setPixmap(QPixmap(":/images/error.png")); // 假设你有一个错误图片资源
            }
        }
        event->acceptProposedAction();
    }

private:
    QLabel *imageLabel; // 用于显示图片的QLabel
};

#include "main.moc"

int main(int argc, char *argv[]) {
    QApplication app(argc, argv);

    ImageWidget widget;
    widget.setWindowTitle("Drag and Drop Image Viewer");
    widget.show();

    return app.exec();
}
```

`dragEnterEvent` 和 `dropEvent` 函数被重写，以便处理拖拽事件。当有文件被拖拽到窗口时，`dragEnterEvent` 会检查是否有URLs被拖拽进来，如果有，它会接受这个拖拽操作。`dropEvent` 函数会获取拖拽的文件路径，并尝试加载图片。