


缩略图列表例子：

继承QListView
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
  
    void addThumbnail(const QImage &image);  
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