## OVERVIEW
网络服务设计成分层架构，总共分为6层。
- [Layer0](#Layer0) 是对底层socket服务的跨平台封装。
- [Layer1](#Layer1) 提供发送缓冲队列FIFO，接收缓存，消息包队列FIFO，IO工作线程池服务。
- Layer2 向高层隐藏低层操作L0的接口。
- [Layer3](#Layer3) 管理Callback，分派消息包（依赖L1的消息包队列）。

Layer4，Layer5，Layer6，依赖L3，提供不同范围的联网服务。

[Layer4](#Layer4) 提供进程内的名字服务。(旧NS使用，现在不使用)

[Layer5](#Layer5) 提供基于NS服务的联网名字服务。

[Layer6](#Layer6) 提供进程间模块联网服务。

L1~L6有一个重要接口，update，在主线程循环中分派。L1负责努力发送blocking的缓冲队列，L3负责分派消息包。

L5，L6对每一个L3连接设置固定的回调方法，由L3回调L5，L6的回调方法。
```
nel/src/net/module_gateway_transport.cpp|229|cbs->setDefaultCallback(cbDispatchMessage);|
nel/src/net/module_gateway_transport.cpp|677|route->CallbackClient.setDefaultCallback(cbDispatchMessage);|
nel/src/net/unified_network.cpp|601|_CbServer->setDefaultCallback(uncbMsgProcessing);|
nel/src/net/unified_network.cpp|861|cbc->setDefaultCallback(uncbMsgProcessing);|
```

## Layer0
* CSock
* CTcpSock
* CUdpSock

[\[details\]](../../src/doc/nel/net/Layer0.md)

## Layer0.5
* CBufSock 
* CNonBlockingBufSock (extended CBufSock)
* CServerBufSock (extended CNonBlockingBufSock)

CBufSock: send buffering FIFO 数据发送缓冲队列

CNonBlockingBufSock: NoDelay and receiving buffer

[\[details\]](../../src/doc/nel/net/Layer0.5.md)

## Layer1
* CBufClient
* CBufServer

CClientReceiveTask, CServerReceiveTask 数据接收线程池。

[\[details\]](../../src/doc/nel/net/Layer1.md)

## Layer3
* CCallbackClient 
* CCallbackServer 
```c++
CCallbackClient client;
client.addCallbackArray( CallbackArray, sizeof(CallbackArray)/sizeof(CallbackArray[0]) );
client.connect( CInetAddress( "localhost" , 37000 ) );
client.send( msg );
```
```c++
CCallbackServer *server = IService::getInstance()->getServer();
CMessage msgout( "PONG" );
msgout.serial( counter );
server->send( msgout, clientfrom );
```
[\[details\]](../../src/doc/nel/net/Layer3.md)

## Layer4
* CNetManager 
    - manage connection of client or server
    
```c++
CNetManager::send( "PS", msgout, from );
CNetManager::send( "FS", msgout, clientfrom );
```
```c++
CNetManager::addClient( SVC, "localhost:37000" );
CNetManager::addCallbackArray( SVC, CallbackArray, sizeof(CallbackArray)/sizeof(CallbackArray[0]) );
```
[\[details\]](../../src/doc/nel/net/Layer4.md)

## Layer5
* CUnifiedNetwork 
    - service adapter depend to namingservice
    
```c++
CUnifiedNetwork::getInstance()->send("GPMS", msgout);
``` 

named services manager.

L4 L3 you should specify the server of service to a client.

L4 would handle the (re)connection.

L3 you need to connect to server.

由命名服务统一分配端口，自动维护与已经注册在命名服务器的其它服务的连接，所有L5管理的L3连接回调L5的回调方法。

[\[details\]](../../src/doc/nel/net/Layer5.md)

## Layer6
* CStandardGateway
    - CGatewayTransport
    - CGatewayRouter

module messages exchange。

plugged module manager.

gateway相互间自动联网，为这个联网上的不同进程间的模块提供消息路由服务。

[\[details\]](../../src/doc/nel/net/Gateway.md)
