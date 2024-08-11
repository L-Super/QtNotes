
## 示例

降低图像亮度
```cpp
QImage decreaseBrightness(const QImage &image, int value)  
{  
    if (image.isNull())  
        return {};  
    QImage re = image.copy();  
    // 降低图像的亮度  
    for (int y = 0; y < image.height(); ++y)  
    {  
        for (int x = 0; x < image.width(); ++x)  
        {  
            QColor color = image.pixelColor(x, y);  
            int newR = qMax(color.red() - value, 0);  
            int newG = qMax(color.green() - value, 0);  
            int newB = qMax(color.blue() - value, 0);  
            re.setPixelColor(x, y, QColor(newR, newG, newB));  
        }  
    }  
    return re;  
}
```

填充 png 透明图层：
```cpp
void fillTransparentImage(QImage &image, const QColor &color)  
{  
    if (!image.hasAlphaChannel())  
        return;  
    // QImage的扫描行是以从上到下的顺序存储的  
    for (int y = 0; y < image.height(); ++y)  
    {  
        QRgb *scanLine = reinterpret_cast<QRgb *>(image.scanLine(y));  
        for (int x = 0; x < image.width(); ++x)  
        {  
            QRgb pixel = scanLine[x];  
            // 使用alpha值判断是否为透明像素  
            if (qAlpha(pixel) == 0)  
            {  
                // 填充为颜色  
                scanLine[x] = color.rgba();  
            }  
        }  
    }  
}
```

使用 `QPainter` 将 `QImage` 裁剪为圆角图像：
```cpp
QImage rounderRectImage(const QImage &image, int radius)  
{  
    if (image.isNull())  
    {  
        return {};  
    }  
    auto w = image.width();  
    auto h = image.height();  
  
    QImage outImage(w, h, QImage::Format_ARGB32_Premultiplied);  
    outImage.fill(Qt::transparent);  
    QPainter painter(&outImage);  
    painter.setRenderHints(QPainter::Antialiasing | QPainter::SmoothPixmapTransform);  
    // 将图片裁剪为圆角  
    QPainterPath path;  
    QRect rect(0, 0, w, h);  
    path.addRoundedRect(rect, radius, radius);  
    painter.setClipPath(path);  
  
    painter.drawImage(rect, image);  
    return outImage;  
}
```

矫正图像方向：
```cpp
QImage autoRotateImage(const QString &filePath)  
{  
    QImageReader imageReader(filePath);  
  
    // 获取图像方向信息  
    auto orientation = imageReader.transformation();  
  
    // 根据方向信息旋转图像  
    QTransform transform;  
    switch (orientation)  
    {  
    case QImageIOHandler::TransformationMirror:  
        transform.scale(-1, 1); // 镜像  
        break;  
    case QImageIOHandler::TransformationFlip:  
        transform.scale(1, -1); // 垂直翻转  
        break;  
    case QImageIOHandler::TransformationRotate180:  
        transform.rotate(180); // 旋转180度  
        break;  
    case QImageIOHandler::TransformationRotate90:  
        transform.rotate(90); //  旋转90度  
        break;  
    case QImageIOHandler::TransformationMirrorAndRotate90:  
        transform.scale(1, -1).rotate(90); // 垂直翻转顺时针旋转90度  
        break;  
    case QImageIOHandler::TransformationFlipAndRotate90:  
        transform.scale(1, -1).rotate(-90); // 垂直翻转旋转 270度  
        break;  
    case QImageIOHandler::TransformationRotate270:  
        transform.rotate(-90); // 逆时针旋转90度  
        break;  
    default:  
        break;  
    }  
  
    if (transform.isRotating() || transform.isScaling() || transform.isAffine())  
        // 应用变换  
        return imageReader.read().transformed(transform);  
  
    return imageReader.read();  
}
```