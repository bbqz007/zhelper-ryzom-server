## CSock & CTcpSock & CUdpSock L0

## CBufSock <= CNonBlockingBufSock <= CServerBufSock L0.5
CBufSock: send buffering FIFO 数据发送缓冲队列
CNonBlockingBufSock: NoDelay and receiving buffer

## CBufClient & CBufServer L1
CClientReceiveTask, CServerReceiveTask 数据接收线程池。

## CCallbackClient & CCallbackServer L3
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


## CNetManager L4 - manage connection of client or server
```c++
CNetManager::send( "PS", msgout, from );
CNetManager::send( "FS", msgout, clientfrom );
```
```c++
CNetManager::addClient( SVC, "localhost:37000" );
CNetManager::addCallbackArray( SVC, CallbackArray, sizeof(CallbackArray)/sizeof(CallbackArray[0]) );
```


## CUnifiedNetwork L5 - service adapter depend to namingservice
```c++
CUnifiedNetwork::getInstance()->send("GPMS", msgout);
``` 

L4 L3 you should specify the server of service to a client.

L4 would handle the (re)connection.

L3 you need to connect to server.
