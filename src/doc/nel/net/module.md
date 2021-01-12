## OVERVIEW

ModuleManager管理本地Module，Gateway等，Gateway提供Layer6联网服务，Module通过plug到Gateway，可以跟Gateway联网的远端Module交换消息，Gateway负责路由转发这些消息。

同时Gateway提供firewall服务，可以指定Module之间的可见可到达。

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
    - \_ModuleLibraryRegistry
    - \_ModuleFactoryRegistry
    - \_ModuleInstances
    - \_ModuleIds
    - \_ModuleProxyIds
    - \_ModuleSocketsRegistry
    - \_ModuleGatewaysRegistry
* CStandardGateway
    - \_ModuleProxies
    - \_Transports
    - \_Routes

Factory 创建本地Module的工厂类。

Gateway 管理远端Gateway间的连接。

ModuleSocket 管理消息交互分派，Module的plug-in。

MoudleManager 管理Factory，Gateway，ModuleSocket，本地Module。

StandardGateway 即是Gateway也是ModuleSocket，管理远端ModuleProxy。

Module 必须plug-in到一个StandardGateway，才能为远端提供module服务。自动通知联网所有Gateway创建ModuleProxy。

Route 与远端Gateway的一个L3连接

ModuleProxy 远端Module，关联一个Route，通过Route向远端Gateway发送模块消息，远端Gateway向ModuleProxy转发消息直到到达目的地，由Module分派消息。 
