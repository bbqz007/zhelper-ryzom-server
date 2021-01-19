[doc license](../../LICENSE)

## OVERVIEW
nel/georges

* sheet：固有定义格式的XML文件，有一个FORM元素按格式存储数据。
  - \<FORM>
	  - \<STRUCT name>
	  	- \<ATOM name value>
	  - \<ARRAY name>	
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

sheet表格式定义 
* .Dfn文件，包含表格式定义，可以包含其它.Dfn，与其说是表，层级文档更加合适，并非单纯的二维表。
* .Typ文件，类型声明
```
<?xml version="1.0"?>
<DFN Revision="$Revision: 1.9 $" State="modified">
  <ELEMENT Name="Chances" Type="Dfn" Filename="_success_chances_line.dfn" Array="true"/>
  <ELEMENT Name="Max Success" Type="Type" Filename="float.typ"/>
  <ELEMENT Name="Max Success Factor" Type="Type" Filename="int.typ" Default="1.0"/>
  <ELEMENT Name="Max Partial Success Factor" Type="Type" Filename="int.typ" Default="&quot;Max Success Factor&quot;"/>
  <ELEMENT Name="Min Partial Success Factor" Type="Type" Filename="int.typ" Default="&quot;Min Success Factor&quot;"/>
  <ELEMENT Name="Min Success Factor" Type="Type" Filename="int.typ" Default="0.55"/>
  <ELEMENT Name="Full Success Roll" Type="Type" Filename="int.typ" Default="0"/>
  <ELEMENT Name="Min Success Roll" Type="Type" Filename="int.typ" Default="90"/>
  <COMMENTS>Converted from old format</COMMENTS>
  <LOG>Fri May 17 15:24:10 2002 (corvazier) File converted from old format
Thu Mar 25 16:23:09 2004 (fleury) Dfn Structure =
Thu Jun 03 20:40:35 2004 (fleury) Dfn Structure =
Thu Jun 03 20:40:57 2004 (fleury) Dfn Structure =
Tue Sep 07 18:53:34 2004 (fleury) Dfn Structure = </LOG>
</DFN>
```
```
type = class CStaticSuccessTable {
  public:
    std::vector<CSuccessXpLine> _SuccessXpTable;
    float _MaxSuccessFactor;
    float _MaxPartialSuccessFactor;
    float _MinPartialSuccessFactor;
    uint8 _FullSuccessRoll;
    uint8 _MinSuccessRoll;
```
上面分别是sheet表设计与运行的代码类。	

##
packed_sheets对应一个C++数据结构在一种容器内的一组对象数据的流串的序列化，包含这个数据结构的XML定义描述（dfn，typ），包含这个数据结构的XML数据源（form）。

这个数据结构必须实现```serial```使用流串序列化数据到packed_sheets文件，或反序列化从packed_sheets文件数据

这个数据结构必须实现```readGeorges```从XML文件读取更新数据。

调用```readGeorges```的方法通常取名为```readForm```，意为从\<FORM>中读取数据，慢速。

调用```serial```的帮助方法名为```loadForm```，意为从流串反序列化读取数据，快速。

##
datasets.packed_sheets是一个特殊的文件，对应于一组CDataSetSheet对象，应用在Mirror服务的DataSetBase，用于定义数据集，一个数据集对应一个数据实体，一个数据实体包含多个属性。一个数据集特殊地可以看作一个二维表，属性是列定义。

