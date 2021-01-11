## OVERVIEW
接口
* IModule
* IModuleFactory
    - createModule()
* IMoudleManager
* IModuleProxy
* IMoudleGateway
* IModuleSocket
    - sendModuleMessage()
    - dispatchModuleMessage()
    - plugModule()
    - unplugModule()
基础类
* CModuleBase
* CModuleFactory
* CModuleProxy
* CMoudleGateway
* CModuleSocket

管理器
* CMoudleManager 
    - 
    -
    -
* CStandardGateway
    -

Factory 创建本地Module的工厂类。

Gateway 管理远端Gateway间的连接。

ModuleSocket 管理消息交互分派，Module的plug-in。

MoudleManager 管理Factory，Gateway，ModuleSocket，本地Module。

StandardGateway 即是Gateway也是ModuleSocket，管理远端ModuleProxy。

Module 必须plug-in到一个StandardGateway，才能为远端提供module服务。

