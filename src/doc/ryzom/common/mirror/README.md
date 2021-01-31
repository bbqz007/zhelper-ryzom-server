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


## CONCEPTS
DataSet 数据集或表， 一个CSheetId对应一个数据集描述。

Entity与Row 行描述，行数据。

Property 列属性，一个单元数据。

tracker与subscriber 一个意思，订阅的内容可以是Entity（bind，sync，adding，removing），也可以是Property （change）。

delta 一个订阅内容对应的消息，事件发生了。CChangeDeltaMS

tracker与delta可以参看```CMirrorService::processReceivedDelta()```。

## AUTOMATON
CMirrorService有两个源代码，mirror_service.cpp与ms_automaton.cpp。
```c++
/*
 * Description of a the cycle of the Mirror Service and of a client service:
 *
 *				Mirror Service (M)												Client Service (C)
 *
 * 1. Receive master TICK from Tick Service		    TICK
 * |  => Send TICK to local client services     ------------>	1. Receive TICK => call tick update callback
 *																|
 * 2. Receive messages from client services		MSG to forward	|
 * |  => store them in message queue		    <------------	|
 * |  Receive delta updates from remote MSs						|
 * |  => store them in pending deltas			    TOCK		|
 * |  Wait for all TOCKs from clients services  <------------	When done, send TOCK to MS
 * |
 *
 * 3. Build and send delta to remote MSs
 *
 * 3.5 Wait for all delta updates received						X: non-synchronized message callbacks
 *    Receive delta updates of same TICK from
 *    remote MSs that didn't answer before
 *
 *
 * 4. Send master TOCK to Tick Service
 * |
 * 5. Apply pending deltas to shared memory
 * |											     UMM
 * 6. Trigger processing of mirror changes		------------>	2. Execute notification callback to update
 * |  and send messages to client services						|  mirror entities and process changes
 *																|
 *																3. Receive messages and transport classes
 *																|  and execute callbacks
 *
 *																Note: non-mirrored messages and non-mirror
 *																transport classes may be received between
 *																C1. and C2. and between C3. and C1.
 *
 *
 *																X: non-synchronized message callbacks
 *
 * Parallelism and consistency: on one physical machine, the services read and write properties
 * in the same shared memory. It means changing a value by a service A is immediately reflected
 * to the service B if they are on the same physical machine. A side effect is that a value can
 * have changed between two successive readings. In most cases, this is not a problem, for exemple
 * if the property represents a colour or such an unimportant property. However, for certain kinds
 * of properties, it may be dangerous. For example, let's say _MyValue is a CMirrorPropValueRO.
 * The following code may lead to a crash: if (_MyValue() != 0) result=1.0f/_MyValue(); because
 * the value can become 0 at any time. The solution to this is to write: float value = _MyValue();
 * if (value != 0) result=1.0f/value;. A similar problem can occur when storing a dataset row (e.g.
 * for targeting purpose): the result of the test isValid() can change. User should use the
 * temporary copy way.
 */
```
