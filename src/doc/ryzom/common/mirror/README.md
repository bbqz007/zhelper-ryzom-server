[doc license]()

## OVERVIEW

* CSheetId
* CDataSetSheet
* CDataSetBase
    - _derived_, CMirroredDataSet
        - _PropAllocator : CPropertyAllocatorClient*
    - _derived_, CDataSetMS
    
* CMirror
    - _has_, CMirroredDataSet
* CMirrorService
    - _has_, CDataSetMS
    
![img](https://img2020.cnblogs.com/blog/665551/202101/665551-20210118204007831-1667447109.jpg)

镜像数据库的segment布局可以参看```CMirrorService::allocateProperty()```，每一个数据集（或者称作表）行的最大数目是指定好的，每一个属性列分配一个固定空间的segment。

CDataSetSheet是从datasets文件加载的数据集（或称作表）的静态描述。CDataSetBase是数据集数据管理器，是动态的。CMirroredDataSet是（Mirror服务的）客户端的实现，用于CMirror；而CDataSetMS则是（Mirror服务的）服务端的实现，应用于CMirrorService。
