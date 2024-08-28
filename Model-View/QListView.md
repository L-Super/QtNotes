


缩略图列表例子：

`ThumbnailListView` 继承QListView，实现一些自定义属性：
```cpp
// ThumbnailListView.h
#pragma once  
  
#include <QListView>  
  
class ThumbnailListView : public QListView  
{  
    Q_OBJECT  
  public:  
    explicit ThumbnailListView(QWidget *parent = nullptr);  
};
```

```cpp
// ThumbnailListView.cpp
#include "ThumbnailListView.h"  
#include "ThumbnailListModel.h"  

  
ThumbnailListView::ThumbnailListView(QWidget *parent) : QListView(parent)  
{  
    // 所有项目是否具有相同的大小  
    // setUniformItemSizes(true);  
    // SelectionMode模式 选择多个项目  
    setSelectionMode(QAbstractItemView::ExtendedSelection);  
    // 设置项目之间的间距  
    setSpacing(6);  
    // 支持hover态边框样式绘制  
    setMouseTracking(true);  
    // 设置滚动条属性  
    setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOff);  
    setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);  
  
    connect(verticalScrollBar(), &QScrollBar::valueChanged, this, &ThumbnailListView::onScrollBarValueChanged);  
}
```

定义 `ThumbnailItem` 数据结构
```cpp
// ThumbnailItem.h
#pragma once  
  
#include <QImage>  
#include <QMetaType>  
#include <QString>  
namespace raw  
{  
enum ModelEnum  
{  
    ItemLogicID = 0x0101,  
    ItemModify = 0x0102,  
    ItemLoadingStatus = 0x0103,  
    ItemFileStatus = 0x0104,  
};  
  
// Q_ENUMS(ModelEnum);  
  
struct ThumbnailItem  
{  
    enum LoadingStatus  
    {  
        None,  
        Loading,  
        Loaded  
    };  
    enum class FileStatus  
    {  
        None,  
        Exist, // file exists  
        Missing, // file was missing(delete, remove)  
        RenderErr // render raw wrong(not supported format)  
    };  
  
    QImage thumbnail;       // 缩略图  
    QString originRawUri;   // 原始Raw文件路径  
    QString imageUri;       // 转档后的jpg路径  
    QString effectImageUri; // 效果图路径  
    QString logicId; // item ID  
    bool isModify{}; // 是否编辑过  
    LoadingStatus loadingStatus{LoadingStatus::None};  
    FileStatus fileStatus{FileStatus::None};  
};  
} // namespace raw  
  
using ThumbnailItem = raw::ThumbnailItem;  
  
Q_DECLARE_METATYPE(ThumbnailItem);
```
`ThumbnailListModel` 定义 model，实现数据增删插改的操作：
```cpp
// ThumbnailListModel.h
#pragma once  
  
#include "ThumbnailItem.h"  
#include <QAbstractListModel>  
#include <QImage>  
#include <QVector>  
  
class ThumbnailListModel : public QAbstractListModel  
{  
    Q_OBJECT  
  public:  
    explicit ThumbnailListModel(QObject *parent = nullptr);  
  
    int rowCount(const QModelIndex &parent = QModelIndex()) const override;  
    QVariant data(const QModelIndex &index, int role = Qt::DisplayRole) const override;  
  
    void updateThumbnail(int index, const QImage &image);  
  
    void addThumbnailItem(const ThumbnailItem &item);  
    void addThumbnailItem(const QVector<ThumbnailItem> &itemList);  
    void removeThumbnailItem(int index);  
    void updateThumbnailItem(int index, const ThumbnailItem &item);  
    void updateThumbnailItemImage(const QString &itemID, const QImage &image);  
    void updateThumbnailItemLoadingStatus(const QString &itemID, const ThumbnailItem::LoadingStatus &status);  
    void updateThumbnailItemFileStatus(const QString &itemID, const ThumbnailItem::FileStatus &status);  
    void updateThumbnailItemModifyStatus(const QString &itemID, bool status);  
  
    QList<QString> viewableAreaItem(int firstIndex, int lastIndex);  
  
  private:  
    QVector<ThumbnailItem> thumbnails;  
};
```

```cpp
// ThumbnailListModel.cpp
#include "ThumbnailListModel.h"  
#include <QFileInfo>  
  
ThumbnailListModel::ThumbnailListModel(QObject *parent) : QAbstractListModel(parent)  
{  
}  
  
int ThumbnailListModel::rowCount(const QModelIndex &parent) const  
{  
    Q_UNUSED(parent)  
    if (thumbnails.isEmpty())  
        return 0;  
    return thumbnails.size();  
}  
  
QVariant ThumbnailListModel::data(const QModelIndex &index, int role) const  
{  
    if (!index.isValid() || index.row() >= thumbnails.size())  
        return QVariant();  
  
    using namespace raw;  
    const ThumbnailItem &thumbnail = thumbnails[index.row()];  
  
    QVariant value;  
  
    switch (role)  
    {  
    case Qt::DisplayRole: // 文字  
    {  
        value = thumbnail.originRawUri;  
    }  
    break;  
    case Qt::DecorationRole: // 图标  
    {  
        value = thumbnail.thumbnail;  
    }  
    break;  
    case Qt::UserRole: // 用户自定义的数据  
    {  
        // 返回缩略图列表序号  
        int imageIndex = index.row() + 1;  
        value = imageIndex;  
    }  
    break;  
    case Qt::ToolTipRole: // tooltip  
    {  
        QFileInfo fileInfo(thumbnail.originRawUri);  
        value = fileInfo.fileName();  
    }  
    break;  
    case ModelEnum::ItemModify: // 是否编辑过  
    {  
        value = thumbnail.isModify;  
    }  
    break;  
    case ModelEnum::ItemLogicID: // item logicID  
    {  
        value = thumbnail.logicId;  
    }  
    break;  
    case ModelEnum::ItemLoadingStatus: // item loading status  
    {  
        value = thumbnail.loadingStatus;  
    }  
    break;  
    case ModelEnum::ItemFileStatus: // item file status  
    {  
        value = static_cast<int>(thumbnail.fileStatus);  
    }  
    break;  
    default:  
        break;  
    }  
  
    return value;  
}  
  
void ThumbnailListModel::updateThumbnail(int index, const QImage &image)  
{  
    if (index >= 0 && index < thumbnails.size())  
    {  
        thumbnails[index].thumbnail = image;  
        emit dataChanged(createIndex(index, 0), createIndex(index, 0));  
    }  
}  
  
void ThumbnailListModel::addThumbnailItem(const ThumbnailItem &item)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    thumbnails.append(item);  
    endInsertRows();  
}  
  
void ThumbnailListModel::addThumbnailItem(const QVector<ThumbnailItem> &itemList)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    thumbnails.append(itemList);  
    endInsertRows();  
}  
  
void ThumbnailListModel::removeThumbnailItem(int index)  
{  
    if (index < 0 || index >= thumbnails.size())  
        return;  
    beginRemoveRows(QModelIndex(), index, index);  
    thumbnails.remove(index);  
    endRemoveRows();  
}  
  
void ThumbnailListModel::updateThumbnailItem(int index, const ThumbnailItem &item)  
{  
    if (index >= 0 && index < thumbnails.size())  
    {  
        thumbnails[index] = item;  
        emit dataChanged(createIndex(index, 0), createIndex(index, 0));  
    }  
}  
  
void ThumbnailListModel::updateThumbnailItemImage(const QString &itemId, const QImage &image)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    auto item = std::find_if(thumbnails.begin(), thumbnails.end(), [itemId](const auto &thumb) { return thumb.logicId == itemId; });  
    if (item != thumbnails.end())  
    {  
        item->thumbnail = image;  
    }  
    endInsertRows();  
}  
  
QList<QString> ThumbnailListModel::viewableAreaItem(int firstIndex, int lastIndex)  
{  
    if (thumbnails.isEmpty())  
    {  
        return {};  
    }  
  
    int lastIndex_ = thumbnails.count() - 1;  
    if (firstIndex > lastIndex || firstIndex > lastIndex_)  
    {  
        return {};  
    }  
  
    int start = std::min(firstIndex, lastIndex_);  
    int end = std::min(lastIndex, lastIndex_);  
  
    QList<QString> list;  
    for (int i = start; i <= end; ++i)  
    {  
        list.push_back(thumbnails.at(i).logicId);  
    }  
    return list;  
}  
  
void ThumbnailListModel::updateThumbnailItemLoadingStatus(const QString &itemID, const ThumbnailItem::LoadingStatus &status)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    auto item = std::find_if(thumbnails.begin(), thumbnails.end(), [itemID](const auto &thumb) { return thumb.logicId == itemID; });  
    if (item != thumbnails.end())  
    {  
        item->loadingStatus = status;  
    }  
    endInsertRows();  
}  
  
void ThumbnailListModel::updateThumbnailItemFileStatus(const QString &itemID, const ThumbnailItem::FileStatus &status)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    auto item = std::find_if(thumbnails.begin(), thumbnails.end(), [itemID](const auto &thumb) { return thumb.logicId == itemID; });  
    if (item != thumbnails.end())  
    {  
        item->fileStatus = status;  
    }  
    endInsertRows();  
}  
  
void ThumbnailListModel::updateThumbnailItemModifyStatus(const QString &itemID, bool status)  
{  
    beginInsertRows(QModelIndex(), thumbnails.size(), thumbnails.size());  
    auto item = std::find_if(thumbnails.begin(), thumbnails.end(), [itemID](const auto &thumb) { return thumb.logicId == itemID; });  
    if (item != thumbnails.end())  
    {  
        item->isModify = status;  
    }  
    endInsertRows();  
}
```
`ThumbnailListDelegate` 绘制 item 样式：
```cpp
// ThumbnailListDelegate.h
#pragma once  
  
#include "ThumbnailItem.h"  
#include <QStyledItemDelegate>  
  
class ThumbnailListDelegate : public QStyledItemDelegate  
{  
    Q_OBJECT  
  public:  
    explicit ThumbnailListDelegate(QObject *parent = nullptr);  
    void paint(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const override;  
    QSize sizeHint(const QStyleOptionViewItem &option, const QModelIndex &index) const override;  
    bool editorEvent(QEvent *event, QAbstractItemModel *model, const QStyleOptionViewItem &option, const QModelIndex &index) override;  
  
  protected:  
    void drawThumbnail(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const;  
    void drawItemModifyIcon(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const;  
    void drawImageIndex(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const;  
    void drawImageLoadingStatus(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const;  
    void drawImageFileStatus(QPainter *painter, const QStyleOptionViewItem &option, const raw::ThumbnailItem::FileStatus& status) const;  
  
  signals:  
    void createMenuSignal(const QPoint& positon, const QModelIndex& index);  
};
```

```cpp
// ThumbnailListDelegate.cpp
#include "ThumbnailListDelegate.h"  
  
#include <QContextMenuEvent>  
#include <QDebug>  
#include <QEvent>  
#include <QMenu>  
#include <QPainter>  
#include <QPainterPath>  
  
namespace  
{  
const int BorderRadius = 6;  
}  
ThumbnailListDelegate::ThumbnailListDelegate(QObject *parent) : QStyledItemDelegate(parent)  
{  
}  
  
void ThumbnailListDelegate::paint(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    if (!index.isValid())  
        return;  
    QRect rect = option.rect;  
  
    // init QPainter  
    painter->setRenderHint(QPainter::Antialiasing);  
  
    drawThumbnail(painter, option, index);  
    drawItemModifyIcon(painter, option, index);  
  
    auto loadingStatus = static_cast<raw::ThumbnailItem::LoadingStatus>(index.data(raw::ModelEnum::ItemLoadingStatus).toInt());  
    if (loadingStatus == raw::ThumbnailItem::Loading)  
    {  
        drawImageLoadingStatus(painter, option, index);  
    }  
  
    auto fileStatus = static_cast<raw::ThumbnailItem::FileStatus>(index.data(raw::ModelEnum::ItemFileStatus).toInt());  
    drawImageFileStatus(painter, option, fileStatus);  
  
    // showing index after draw file status  
    drawImageIndex(painter, option, index);  
  
    // item selected  
    if (option.state & QStyle::State_Selected)  
    {  
        painter->save();  
        QPen borderPen;  
        borderPen.setColor(QColor(255, 255, 255));  
        borderPen.setWidth(2);  
        painter->setBrush(Qt::NoBrush);  
        painter->setPen(borderPen);  
        painter->drawRoundedRect(rect, BorderRadius, BorderRadius);  
        painter->restore();  
    }  
    // item hover  
    if (option.state & QStyle::State_MouseOver)  
    {  
        painter->save();  
        QPen borderPen;  
        borderPen.setColor(QColor(255, 255, 255, 200));  
        borderPen.setWidth(2);  
        painter->setBrush(Qt::NoBrush);  
        painter->setPen(borderPen);  
        painter->drawRoundedRect(rect, BorderRadius, BorderRadius);  
        painter->restore();  
    }  
}  
  
void ThumbnailListDelegate::drawThumbnail(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    QImage image = index.data(Qt::DecorationRole).value<QImage>();  
    if (image.isNull())  
        return;  
    /// draw thumbnail  
    // 计算绘制区域，减去间距  
    // QRect rect = option.rect.adjusted(5, 5, -5, -5);  
    QRect rect = option.rect;  
  
    // 创建圆角矩形路径  
    QPainterPath path;  
    // 圆角半径  
    path.addRoundedRect(rect, BorderRadius, BorderRadius);  
  
    painter->save();  
    // 设置裁剪区域为圆角矩形  
    painter->setClipPath(path);  
    painter->setRenderHint(QPainter::SmoothPixmapTransform);  
    // 绘制图片  
    painter->drawImage(rect, image);  
    painter->restore();  
}  
  
void ThumbnailListDelegate::drawItemModifyIcon(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    auto isModify = index.data(raw::ModelEnum::ItemModify).toBool();  
    if (!isModify)  
        return;  
    QRect rect = option.rect;  
  
    // 右下角修改标  
    int iconWidth = 16;  
    int iconHeight = 16;  
    QRect iconRect(rect.x(), rect.y(), iconWidth, iconHeight);  
    QImage icon(":/Image/yunxiu/Image/imgParamChangedFlag.png");  
    icon = icon.scaled(iconWidth, iconHeight, Qt::KeepAspectRatio, Qt::SmoothTransformation);  
  
    painter->save();  
    painter->setRenderHint(QPainter::SmoothPixmapTransform);  
    painter->drawImage(iconRect, icon);  
    painter->restore();  
}  
  
void ThumbnailListDelegate::drawImageIndex(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    QRect rect = option.rect;  
    int thumbnailIndex = index.data(Qt::UserRole).toInt();  
  
    int labelWidth = 20;  
    int labelHeight = 16;  
    if (thumbnailIndex >= 100 && thumbnailIndex < 1000)  
    {  
        labelWidth = 30;  
    }  
    else if (thumbnailIndex >= 1000 && thumbnailIndex < 10000)  
    {        labelWidth = 40;  
    }  
    else if (thumbnailIndex >= 10000 && thumbnailIndex < 100000)  
    {  
        labelWidth = 50;  
    }  
  
    int cornerX = rect.x();  
    int cornerY = rect.y() + rect.height() - labelHeight;  
    int radius = 12;  
  
    QPainterPath roundedRect;  
    // 移动到起点  
    roundedRect.moveTo(cornerX, cornerY);  
    // 绘制左侧边框  
    roundedRect.lineTo(cornerX, cornerY + labelHeight - radius);  
    // 绘制左下角圆角  
    roundedRect.arcTo(cornerX, cornerY + labelHeight - radius, radius, radius, 180, 90);  
    // 绘制底部边框  
    roundedRect.lineTo(cornerX + labelWidth, cornerY + labelHeight);  
    // 绘制右侧边框  
    roundedRect.lineTo(cornerX + labelWidth, cornerY + radius);  
    // 绘制右上角圆角  
    roundedRect.arcTo(cornerX + labelWidth - radius, cornerY, radius, radius, 0, 90);  
    roundedRect.closeSubpath();  
  
    painter->save();  
    // 设置为Qt::NoPen，不显示路径的边界线。然后，使用QPainter::setBrush填充颜色  
    painter->setPen(Qt::NoPen);  
    painter->setBrush(QColor("#99000000"));  
    painter->drawPath(roundedRect);  
    painter->restore();  
  
    // draw index text  
    painter->save();  
    QColor textColor(Qt::white);  
    textColor.setAlphaF(0.8);  
    painter->setPen(textColor);  
    painter->setBrush(Qt::NoBrush);  
    painter->drawText(cornerX, cornerY, labelWidth, labelHeight, Qt::AlignCenter, QString::number(thumbnailIndex));  
    painter->restore();  
}  
  
void ThumbnailListDelegate::drawImageLoadingStatus(QPainter *painter, const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    Q_UNUSED(index)  
    QRect rect = option.rect;  
    QColor bgColor = Qt::black;  
    bgColor.setAlphaF(0.5);  
    painter->save();  
    painter->setBrush(bgColor);  
    painter->setPen(Qt::NoPen);  
  
    QPainterPath rectPath;  
    rectPath.addRoundedRect(rect, 6, 6);  
    QPainterPath circlePath;  
    circlePath.addEllipse(rect.center(), 30, 30);  
    painter->drawPath(rectPath - circlePath);  
  
    QRect pieRect;  
    pieRect.setSize(QSize(52, 52));  
    pieRect.moveCenter(rect.center());  
    painter->drawPie(pieRect, 90 * 16, 3 * 360 * 16 / 8);  
  
    painter->restore();  
}  
  
void ThumbnailListDelegate::drawImageFileStatus(QPainter *painter, const QStyleOptionViewItem &option, const raw::ThumbnailItem::FileStatus &status) const  
{  
    using FileStatus = raw::ThumbnailItem::FileStatus;  
    if (status == FileStatus::None || status == FileStatus::Exist)  
        return;  
    QRect rect = option.rect;  
  
    /// fill background  
    painter->save();  
    QColor color(48,48,51);  
    painter->setBrush(QBrush(color));  
    painter->setPen(Qt::NoPen);  
    painter->drawRoundedRect(rect, BorderRadius, BorderRadius);  
    painter->restore();  
  
    painter->save();  
    painter->setPen(QColor("#CBCCCF"));  
    painter->setBrush(Qt::NoBrush);  
  
    switch (status)  
    {  
    case FileStatus::Missing:  
        painter->drawText(rect, Qt::AlignCenter, tr("源文件不存在"));  
        break;  
    case FileStatus::RenderErr:  
        painter->drawText(rect, Qt::AlignCenter, tr("格式不支持"));  
        break;  
    default:  
        break;  
    }  
  
    painter->restore();  
}  
  
QSize ThumbnailListDelegate::sizeHint(const QStyleOptionViewItem &option, const QModelIndex &index) const  
{  
    Q_UNUSED(option);  
    Q_UNUSED(index);  
    // 返回项目的大小，添加间距  
    int itemWidth = 90;  
    int itemHeight = 90;  
    int spacing = 0;  
    // 假设每个缩略图项目为90x90  
    return {itemWidth + spacing, itemHeight + spacing};  
}  
  
bool ThumbnailListDelegate::editorEvent(QEvent *event, QAbstractItemModel *model, const QStyleOptionViewItem &option, const QModelIndex &index)  
{  
    if (event->type() == QEvent::MouseButtonPress)  
    {  
        auto mouseEvent = dynamic_cast<QMouseEvent *>(event);  
  
        if (mouseEvent && mouseEvent->button() == Qt::RightButton)  
        { // mouse right-click  
            emit createMenuSignal(mouseEvent->globalPos(), index);  
            return true;  
        }  
    }  
    if (event->type() == QEvent::MouseButtonDblClick)  
    {  
        qDebug() << "double click";  
    }  
    return QStyledItemDelegate::editorEvent(event, model, option, index);  
}
```
`ThumbnailListWidget` 组合 model-view 结构：
```cpp
// ThumbnailListWidget.h
#pragma once  
  
#include "ThumbnailItem.h"  
#include <QItemSelection>  
#include <QFrame>  
  
class ThumbnailListView;  
class ThumbnailListModel;  
class ThumbnailListDelegate;  
class ThumbnailListWidget : public QFrame  
{  
    Q_OBJECT  
  public:  
    explicit ThumbnailListWidget(QWidget *parent = nullptr);  
  
    bool addThumbnailItem(const ThumbnailItem &item);  
    bool addThumbnailItems(const QVector<ThumbnailItem> &itemList);  
  
    void updateThumbnailItemImage(const QString &logicID, const QImage &image);  
    void updateThumbnailItemLoadingStatus(const QString &itemID, const ThumbnailItem::LoadingStatus &status);  
    void updateThumbnailItemFileStatus(const QString &itemID, const ThumbnailItem::FileStatus &status);  
    void updateThumbnailItemModifyStatus(const QString &itemID, bool status);  
  
    /**  
     * Get selected item in listview     * @return ID list     */    QVector<QString> selectedThumbnailItems();    
  
  protected:  
    /**  
     * Set the currently selected item.     * @param row     * @param sendSignal     */    void setHighlightItem(int row, bool sendSignal);  
  
  protected:  
    void enterEvent(QEvent *event) override;  
    void leaveEvent(QEvent *event) override;  
  
  private slots:  
    void onItemSelected(const QItemSelection &selected, const QItemSelection &deselected);  
    void onCopyParameters(const QModelIndex &index);  
    void onPasteParameters(const QModelIndex &index);  
    void onRemoveImage(const QModelIndex &index);  
    void onCurrentItemChanged(const QModelIndex &current, const QModelIndex &previous);  
    void onContextMenu(const QPoint& position, const QModelIndex& index);  
  
  signals:  
    void updatePreviewAreaImageSignal(const QString& logicID); // 通知预览区切换图片信号  
    void viewableAreaChangedSignal(const QList<QString> &logicIDList); // 可视区域改变发送可见item  
    void copyParameterSignal(const QString& ID); // 参数复制信号  
    void pasteParameterSignal(const QString& fromID, const QString& toID); // 参数粘贴信号  
    void removeThumbnailItemsSignal(const QVector<QString>& items); // 移除item时，发送ID列表  
  
  private:  
    ThumbnailListView *listView;  
    ThumbnailListModel *model;  
    ThumbnailListDelegate *delegate;  
    QString copySrcLogicID; // 右键菜单复制参数项的ID  
};
```

```cpp
// ThumbnailListWidget.cpp
#include "ThumbnailListWidget.h"  
#include "ThumbnailListDelegate.h"  
#include "ThumbnailListModel.h"  
#include "ThumbnailListView.h"  
#include "UI/Component/MToast.h"  
  
#include <QDebug>  
#include <QHBoxLayout>  
#include <QImage>  
#include <QMenu>  
  
ThumbnailListWidget::ThumbnailListWidget(QWidget *parent)  
    : QFrame{parent}, listView(new ThumbnailListView(this)), model(new ThumbnailListModel(this)), delegate(new ThumbnailListDelegate(this))  
{  
    setObjectName("ThumbnailListWidget");  
  
    listView->setModel(model);  
    listView->setItemDelegate(delegate);  
  
    connect(delegate, &ThumbnailListDelegate::createMenuSignal, this, &ThumbnailListWidget::onContextMenu);  
  
    // 当选择项变化时  
    connect(listView->selectionModel(), &QItemSelectionModel::selectionChanged, this, &ThumbnailListWidget::onItemSelected);  
    // 当当前项变化时  
    connect(listView->selectionModel(), &QItemSelectionModel::currentChanged, this, &ThumbnailListWidget::onCurrentItemChanged);  
    // 可视区域item数  
    connect(listView, &ThumbnailListView::viewableAreaChangedSignal, this, &ThumbnailListWidget::viewableAreaChangedSignal);  
  
    auto layout = new QHBoxLayout();  
    layout->setContentsMargins(4, 4, 4, 4);  
    layout->addWidget(listView);  
    setLayout(layout); 
}  
  
bool ThumbnailListWidget::addThumbnailItem(const ThumbnailItem &item)  
{  
    model->addThumbnailItem(item);  
    setHighlightItem(0, false);  
    return false;  
}  
  
bool ThumbnailListWidget::addThumbnailItems(const QVector<ThumbnailItem> &itemList)  
{  
    model->addThumbnailItem(itemList);  
    setHighlightItem(0, false);  
    return false;  
}  
  
void ThumbnailListWidget::updateThumbnailItemImage(const QString &itemId, const QImage &image)  
{  
    model->updateThumbnailItemImage(itemId, image);  
}  
  
void ThumbnailListWidget::updateThumbnailItemLoadingStatus(const QString &itemID, const ThumbnailItem::LoadingStatus &status)  
{  
    model->updateThumbnailItemLoadingStatus(itemID, status);  
}  
  
void ThumbnailListWidget::updateThumbnailItemFileStatus(const QString &itemID, const ThumbnailItem::FileStatus &status)  
{  
    model->updateThumbnailItemFileStatus(itemID, status);  
}  
  
void ThumbnailListWidget::updateThumbnailItemModifyStatus(const QString &itemID, bool status)  
{  
    model-> updateThumbnailItemModifyStatus(itemID, status);  
}  
  
QVector<QString> ThumbnailListWidget::selectedThumbnailItems()  
{  
    QItemSelectionModel *selectionModel = listView->selectionModel();  
    QModelIndexList selectedIndexes = selectionModel->selectedIndexes();  
    QVector<QString> items;  
    foreach (const auto &item, selectedIndexes)  
    {  
        items.append(item.data(raw::ItemLogicID).toString());  
    }  
    return items;  
}  
  
void ThumbnailListWidget::onCopyParameters(const QModelIndex &index)  
{   
    copySrcLogicID = index.data(raw::ItemLogicID).toString();  
    emit copyParameterSignal(copySrcLogicID);  
}  
  
void ThumbnailListWidget::onPasteParameters(const QModelIndex &index)  
{   
    auto currentID = index.data(raw::ItemLogicID).toString();  
    emit pasteParameterSignal(copySrcLogicID, currentID);  
}  
  
void ThumbnailListWidget::onRemoveImage(const QModelIndex &index)  
{  
    if (!index.isValid())  
        return;  
  
    auto isDelete = [this](auto size) {  
        bool confirmDelete{false};  
        QString title = tr("移除 %1 张图片").arg(size);  
  
        MToast::confirm(title, tr("仅将图片从美图云修中移除，不影响原文件。"), [&confirmDelete]() { confirmDelete = true; }, this, tr("确认"));  
        if (confirmDelete)  
            return true;  
        return false;  
    };  
  
    // 获取当前选中的项的索引  
    int row = index.row();  
    QVector<QString> items;  
    // 获取当前所有被选中的索引项  
    QItemSelectionModel *selectionModel = listView->selectionModel();  
    QModelIndexList selectedIndexes = selectionModel->selectedIndexes();  
  
    // 检查右键点击的项是否已被选中  
    bool isIndexSelected = selectedIndexes.contains(index);  
  
    if (isIndexSelected)  
    {  
        if (!isDelete(selectedIndexes.size()))  
        {  
            return;  
        }  
  
        // 开始从后往前删除选中的项，以避免索引冲突  
        std::sort(selectedIndexes.begin(), selectedIndexes.end(), [](const QModelIndex &a, const QModelIndex &b) { return a.row() > b.row(); });  
  
        for (const QModelIndex &selectedIndex : selectedIndexes)  
        {  
            auto id = selectedIndex.data(raw::ItemLogicID).toString();  
            items.append(id);  
            model->removeThumbnailItem(selectedIndex.row());  
        }  
    }  
    else  
    {  
        if (!isDelete(1))  
            return;  
        // 如果点击的项未被选中，只删除右键菜单点击的项  
        auto id = index.data(raw::ItemLogicID).toString();  
        items.append(id);  
        // 如果没有选中的项，则删除右键点击的项目  
        model->removeThumbnailItem(index.row());  
    }  
    emit removeThumbnailItemsSignal(items);  
    qDebug() << __func__ << "removed items" << items;  
  
    // 非选中项，且已选项只有一个，不做当前选中项切换  
    if(!isIndexSelected && selectedIndexes.size() == 1)  
        return;  
  
    // 确保列表中仍然有项  
    if (model->rowCount() > 0)  
    {  
        // 如果删除的是最后一项，选中前一项  
        if (row >= model->rowCount())  
        {  
            row = model->rowCount() - 1;  
        }  
  
        // 设置新的选中项  
        setHighlightItem(row, isIndexSelected);  
    }  
}  
  
void ThumbnailListWidget::onContextMenu(const QPoint &position, const QModelIndex &index)  
{  
    QMenu menu;  
    QAction *copyAction = menu.addAction("复制参数");  
    QAction *pasteAction = menu.addAction("粘贴参数");  
    QAction *removeAction = menu.addAction("移除图片");  
  
    // 已做效果可以启用复制  
    auto isEnable = index.data(raw::ItemModify).toBool();  
    copyAction->setEnabled(isEnable);  
  
    auto currentID = index.data(raw::ItemLogicID).toString();  
    // 已点击复制且不是源复制项  
    isEnable = !copySrcLogicID.isEmpty() && copySrcLogicID != currentID;  
    pasteAction->setEnabled(isEnable);  
  
    QAction *selectedAction = menu.exec(position);  
    if (selectedAction == copyAction)  
    {  
        onCopyParameters(index);  
    }  
    else if (selectedAction == pasteAction)  
    {        onPasteParameters(index);  
    }  
    else if (selectedAction == removeAction)  
    {  
        onRemoveImage(index);  
    }  
}  
  
void ThumbnailListWidget::onItemSelected(const QItemSelection &selected, const QItemSelection &deselected)  
{  
    Q_UNUSED(selected)  
    Q_UNUSED(deselected)  
    // refresh selected item style (need it)  
    listView->viewport()->update();  
}  
  
void ThumbnailListWidget::onCurrentItemChanged(const QModelIndex &current, const QModelIndex &previous)  
{  
    qDebug() << __func__ << "current" << current.data(Qt::DisplayRole).toString() << "previous" << previous.data(Qt::DisplayRole).toString();  
  
    auto logicID = current.data(raw::ItemLogicID).toString();  
    emit updatePreviewAreaImageSignal(logicID);  
}  
  
void ThumbnailListWidget::enterEvent(QEvent *event)  
{  
    // Incomprehensible product requirements  
    if (listView->verticalScrollBarPolicy() == Qt::ScrollBarAlwaysOff)  
        listView->setVerticalScrollBarPolicy(Qt::ScrollBarAsNeeded);  
    QWidget::enterEvent(event);  
}  
  
void ThumbnailListWidget::leaveEvent(QEvent *event)  
{  
    // Incomprehensible product requirements  
    listView->setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOff);  
    QWidget::leaveEvent(event);  
}  
  
void ThumbnailListWidget::setHighlightItem(int row, bool sendSignal)  
{  
    if (model->rowCount() <= 0)  
        return;  
  
    QModelIndex index = model->index(row, 0);  
    listView->setCurrentIndex(index);  
    listView->selectionModel()->select(index, QItemSelectionModel::Select);  
    if (sendSignal)  
    {   
        emit updatePreviewAreaImageSignal(index.data(raw::ItemLogicID).toString());  
    }  
}
```