## OVERVIEW
接口
* IModule
    - plugModule()
    - unplugModule()
    - onReceiveModuleMessage()
* IModuleFactory
    - createModule()
* IMoudleManager
* IModuleProxy
* IMoudleGateway
    - createTransport()
    - sendModuleMessage()
    - dispatchModuleMessage()
    - onReceiveMessage()
* IModuleSocket
    - sendModuleMessage()
    - broadcastModuleMessage()
    - onModulePlugged()
    - onModuleUnplugged()

    
基础类
* CModuleBase
* CModuleFactory
* CModuleProxy
* CMoudleGateway
* CModuleSocket
    - \_PluggedModules

管理器
* CMoudleManager 
    - 1
    - 2
    - 3
* CStandardGateway
    - 1

Factory 创建本地Module的工厂类。

Gateway 管理远端Gateway间的连接。

ModuleSocket 管理消息交互分派，Module的plug-in。

MoudleManager 管理Factory，Gateway，ModuleSocket，本地Module。

StandardGateway 即是Gateway也是ModuleSocket，管理远端ModuleProxy。

Module 必须plug-in到一个StandardGateway，才能为远端提供module服务。

