[\[doc lisence\]](../../../../LICENSE)

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

## Snap
```
127.0.0.1/AES-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/AES-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/AES-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/AES-0 : Creating module 'aes' of class 'AdminExecutorService' 
127.0.0.1/AES-0 : Creating module 'asc_gw' of class 'StandardGateway' 
127.0.0.1/AES-0 : Creating module 'aes_gw' of class 'StandardGateway' 
127.0.0.1/BS-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/BS-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/BS-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/EGS-131 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/EGS-131 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/EGS-131 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/EGS-131 : Creating module 'glob_gw' of class 'StandardGateway' 
127.0.0.1/EGS-131 : Creating module 'lgs_gw' of class 'StandardGateway' 
127.0.0.1/EGS-131 : Creating module 'suc' of class 'ShardUnifierClient' 
127.0.0.1/EGS-131 : Creating module 'ccf' of class 'ClientCommandForwader' 
127.0.0.1/EGS-131 : Creating module 'cc' of class 'CharacterControl' 
127.0.0.1/EGS-131 : Creating module 'gu' of class 'GuildUnifier' 
127.0.0.1/EGS-131 : Creating module 'cnmc' of class 'CharNameMapperClient' 
127.0.0.1/EGS-131 : Creating module 'lsc' of class 'LoggerServiceClient' 
127.0.0.1/EGS-131 : Creating module 'asm' of class 'AnimSessionManager' 
127.0.0.1/GPMS-128 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/GPMS-128 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/GPMS-128 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/IOS-130 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/IOS-130 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/IOS-130 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/IOS-130 : Creating module 'glob_gw' of class 'StandardGateway' 
127.0.0.1/IOS-130 : Creating module 'lgs_gw' of class 'StandardGateway' 
127.0.0.1/IOS-130 : Creating module 'cuc' of class 'ChatUnifierClient' 
127.0.0.1/IOS-130 : Creating module 'lsc' of class 'LoggerServiceClient' 
127.0.0.1/IOS-130 : Creating module 'cnm' of class 'CharNameMapper' 
127.0.0.1/IOS-130 : Creating module 'iosrm' of class 'IOSRingModule' 
127.0.0.1/NS-1 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/NS-1 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/NS-1 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/WS-129 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/WS-129 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/WS-129 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/WS-129 : Creating module 'ws' of class 'WelcomeService' 
127.0.0.1/TICKS-132 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/TICKS-132 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/TICKS-132 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/MS-133 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/MS-133 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/MS-133 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/AIS-134 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/AIS-134 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/AIS-134 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/MFS-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/MFS-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/MFS-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/SU-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/SU-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/SU-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/SU-0 : Creating module 'glob_gw' of class 'StandardGateway' 
127.0.0.1/SU-0 : Creating module 'rsm' of class 'RingSessionManager' 
127.0.0.1/SU-0 : Creating module 'ls' of class 'LoginService' 
127.0.0.1/SU-0 : Creating module 'cs' of class 'CharacterSynchronisation' 
127.0.0.1/SU-0 : Creating module 'el' of class 'EntityLocator' 
127.0.0.1/SU-0 : Creating module 'mfnfwd' of class 'MailForumNotifierFwd' 
127.0.0.1/SU-0 : Creating module 'cus' of class 'ChatUnifierServer' 
127.0.0.1/SU-0 : Creating module 'ce' of class 'CommandExecutor' 
127.0.0.1/FS-135 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/FS-135 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/FS-135 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/SBS-136 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/SBS-136 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/SBS-136 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/SBS-136 : Creating module 'sbs' of class 'SessionBrowserServerMod' 
127.0.0.1/LGS-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/LGS-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/LGS-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/LGS-0 : Creating module 'ls' of class 'LoggerService' 
127.0.0.1/LGS-0 : Creating module 'lgs_gw' of class 'StandardGateway' 
127.0.0.1/AS-0 : Creating module 'gw' of class 'StandardGateway' 
127.0.0.1/AS-0 : Creating module 'gw_aes' of class 'StandardGateway' 
127.0.0.1/AS-0 : Creating module 'aes_client' of class 'AdminExecutorServiceClient' 
127.0.0.1/AS-0 : Creating module 'as' of class 'AdminService' 
127.0.0.1/AS-0 : Creating module 'as_gw' of class 'StandardGateway' 

```
