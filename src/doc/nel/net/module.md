[\[doc lisence\]](../../../../LICENSE)

## OVERVIEW

ModuleManager管理本地Module，Gateway等，Gateway提供Layer6联网服务，Module通过plug到Gateway，可以跟Gateway联网的远端Module交换消息，Gateway负责路由转发这些消息。

同时Gateway提供firewall服务，可以指定Module之间的可见可到达。

另外，Module借助Gateway，提供RPC中间层服务，由工具生成ClientProxy代码跟ServerSkel代码，可以支持跨平台RPC。

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
* IModuleInterceptable
    - onProcessModuleMessage()
    
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

## RPC
RPC中间层服务。

接口
* IModuleInterceptable
    - onProcessModuleMessage
基础类
* CInterceptorForwarder
* CModuleTracker

使用拦截器模式，借助Gateway分派模块消息的服务，提供RPC中间层服务。

机器生成＜Module＞_itf.h，＜Module＞_itf.cpp，＜Module＞_itf.xml，＜Module＞_itf.php。

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

## RPC SNAP
```
../nelns/welcome_service/welcome_service_itf.cpp
../nelns/welcome_service/welcome_service_itf.h
../ryzom/common/src/game_share/character_sync_itf.cpp
../ryzom/common/src/game_share/character_sync_itf.h
../ryzom/common/src/game_share/character_sync_itf.xml
../ryzom/common/src/game_share/r2_modules_itf.cpp
../ryzom/common/src/game_share/r2_modules_itf.h
../ryzom/common/src/game_share/r2_modules_itf.xml
../ryzom/common/src/game_share/r2_share_itf.cpp
../ryzom/common/src/game_share/r2_share_itf.h
../ryzom/common/src/game_share/r2_share_itf.xml
../ryzom/common/src/game_share/ring_session_manager_itf.cpp
../ryzom/common/src/game_share/ring_session_manager_itf.h
../ryzom/common/src/game_share/ring_session_manager_itf.php
../ryzom/common/src/game_share/ring_session_manager_itf.xml
../ryzom/common/src/game_share/welcome_service_itf.cpp
../ryzom/common/src/game_share/welcome_service_itf.h
../ryzom/server/src/admin_modules/admin_modules_itf.cpp
../ryzom/server/src/admin_modules/admin_modules_itf.h
../ryzom/server/src/admin_modules/admin_modules_itf.xml
../ryzom/server/src/entities_game_service/guild_manager/guild_unifier_itf.cpp
../ryzom/server/src/entities_game_service/guild_manager/guild_unifier_itf.h
../ryzom/server/src/entities_game_service/guild_manager/guild_unifier_itf.xml
../ryzom/server/src/general_utilities_service/re_module_itf.cpp
../ryzom/server/src/general_utilities_service/re_module_itf.h
../ryzom/server/src/general_utilities_service/re_module_itf.xml
../ryzom/server/src/general_utilities_service/rr_module_itf.cpp
../ryzom/server/src/general_utilities_service/rr_module_itf.h
../ryzom/server/src/general_utilities_service/rr_module_itf.xml
../ryzom/server/src/patchman_service/module_admin_itf.cpp
../ryzom/server/src/patchman_service/module_admin_itf.h
../ryzom/server/src/patchman_service/module_admin_itf.xml
../ryzom/server/src/patchman_service/re_module_itf.cpp
../ryzom/server/src/patchman_service/re_module_itf.h
../ryzom/server/src/patchman_service/re_module_itf.xml
../ryzom/server/src/patchman_service/rr_module_itf.cpp
../ryzom/server/src/patchman_service/rr_module_itf.h
../ryzom/server/src/patchman_service/rr_module_itf.xml
../ryzom/server/src/patchman_service/spa_module_itf.cpp
../ryzom/server/src/patchman_service/spa_module_itf.h
../ryzom/server/src/patchman_service/spa_module_itf.xml
../ryzom/server/src/patchman_service/spm_module_itf.cpp
../ryzom/server/src/patchman_service/spm_module_itf.h
../ryzom/server/src/patchman_service/spm_module_itf.xml
../ryzom/server/src/patchman_service/spt_module_itf.cpp
../ryzom/server/src/patchman_service/spt_module_itf.h
../ryzom/server/src/patchman_service/spt_module_itf.xml
../ryzom/server/src/server_share/backup_service_itf.cpp
../ryzom/server/src/server_share/backup_service_itf.h
../ryzom/server/src/server_share/backup_service_itf.xml
../ryzom/server/src/server_share/char_name_mapper_itf.cpp
../ryzom/server/src/server_share/char_name_mapper_itf.h
../ryzom/server/src/server_share/char_name_mapper_itf.xml
../ryzom/server/src/server_share/chat_unifier_itf.cpp
../ryzom/server/src/server_share/chat_unifier_itf.h
../ryzom/server/src/server_share/chat_unifier_itf.xml
../ryzom/server/src/server_share/command_executor_itf.cpp
../ryzom/server/src/server_share/command_executor_itf.h
../ryzom/server/src/server_share/command_executor_itf.xml
../ryzom/server/src/server_share/entity_locator_itf.cpp
../ryzom/server/src/server_share/entity_locator_itf.h
../ryzom/server/src/server_share/entity_locator_itf.xml
../ryzom/server/src/server_share/logger_service_itf.cpp
../ryzom/server/src/server_share/logger_service_itf.h
../ryzom/server/src/server_share/logger_service_itf.xml
../ryzom/server/src/server_share/login_service_itf.cpp
../ryzom/server/src/server_share/login_service_itf.h
../ryzom/server/src/server_share/login_service_itf.php
../ryzom/server/src/server_share/login_service_itf.xml
../ryzom/server/src/server_share/mail_forum_itf.cpp
../ryzom/server/src/server_share/mail_forum_itf.h
../ryzom/server/src/server_share/mail_forum_itf.xml

```

## php RPC SNAP
```
../web/public_php/admin/nel/admin_modules_itf.php
../web/public_php/login/login_service_itf.php
../web/public_php/ring/mail_forum_itf.php
../web/public_php/ring/ring_session_manager_itf.php
../web/public_php/ring/welcome_service_itf.php

```
分别对应的模块服务
```
127.0.0.1/AS-0 : Creating module 'as' of class 'AdminService' 

127.0.0.1/SU-0 : Creating module 'ls' of class 'LoginService' 
127.0.0.1/SU-0 : Creating module 'mfnfwd' of class 'MailForumNotifierFwd' 
127.0.0.1/SU-0 : Creating module 'rsm' of class 'RingSessionManager' 

127.0.0.1/WS-129 : Creating module 'ws' of class 'WelcomeService' 
```
