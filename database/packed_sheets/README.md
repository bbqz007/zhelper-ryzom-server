[doc license](../../LICENSE)

## OVErVIEW
nel/georges

* sheet：固有定义格式的XML文件，有一个FORM元素按格式存储数据。
  - <FORM>
	  - <STRUCT>
* packed_sheets：一个sheet或多个sheet的全部或部分数据加载在一个容器后，形成一个view。按打包格式然后将这个容器系列化后生成的文件。
  - 所有依赖的sheet文件名
  - 当前容器数据view
  
packed_sheets文件相当于一个或多个sheet的view，加载时可能通过参数指定是否需要从sheet重新更新这个view的数据。

```loadFormNoPackedSheet```加载sheet文件。

```loadForm```加载packed_sheets文件。

接口
* UForm
* UFormLoader
* UFormDfn
* UFormElm
* UType

地理表单实现类
* CForm
* CFormLoader
* CFormDfn
* CFormElm
* CType
