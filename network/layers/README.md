## Layer0
* CSock
* CTcpSock
* CUdpSock
[\[details\]]()

## Layer0.5
* CBufSock 
* CNonBlockingBufSock (extended CBufSock)
* CServerBufSock (extended CNonBlockingBufSock)
CBufSock: send buffering FIFO 数据发送缓冲队列

CNonBlockingBufSock: NoDelay and receiving buffer

[\[details\]]()

## Layer1
* CBufClient
* CBufServer
CClientReceiveTask, CServerReceiveTask 数据接收线程池。

[\[details\]]()

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
[\[details\]]()

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
[\[details\]]()

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

[\[details\]]()

## Layer6
* CStandardGateway
    - CGatewayTransport
    - CGatewayRouter

module messages exchange。

plugged module manager.

[\[details\]]()
