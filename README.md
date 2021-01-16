# zhelper-ryzom-server
ryzomcore(open 3d mmorpg game)服务端zhelper版帮助文档。

本文档基于ryzomclassic-develop分支代码，分析，编译，实际运行，从而编写。

### 历史
[dev.ryzom.com](https://dev.ryzom.com)似乎在2012年后停止了，现在访问[wiki.ryzom.dev](https://wiki.ryzom.dev)。

代码仓地址：[github.com/ryzom/ryzomcore](https://github.com/ryzom/ryzomcore)

默认分支：develop （部分代码不完整，在[issue#587](https://github.com/ryzom/ryzomcore/issues/587)可以查到）

选用分支：classic-develop

### 服务器集群
包括16个服务进程，web服务（nginx+php-fpm），mysql（或mariadb）数据库。

* **A(E)S** - ryzom_admin_service - 后台管理控制台

* **bms_master** - backup_service 

* **egs** - entities_game_service - 主逻辑，规则判决

* **gpms** - gpm_service - 地理服务

* **ios** - input_output_service - 聊天，文本信息等

* **rns** - ryzom_naming_service - 后端名字服务

* **rws** - ryzom_welcome_service - 线路，角色登陆服务

* **ts** - tick_service - 世界时间

* **ms** - mirror_service - 内存数据库镜像服务

* **ais_newbyland** - ai_service - NPC，ai

* **mfs** - mail_forum_service 邮箱，留言

* **su** - shard_unifier_service 

* **fes** - frontend_service - 游戏端gateway

* **sbs** - session_browser_server 

* **lgs** - logger_service - 日志服务

* **ras** - ryzom_admin_service 后台管理web端接口服务


### 服务器集群布局
Services Layout
* Domain
    - shard
    
整个游戏服务器包括一个或多个域，每个域由一个或多个世界碎片组成，一个世界碎片使用一台物理机器，通过L5组网。一个域只有一个su，组织域内所有世界碎片。

### index
* **架构**
    - [服务器集群](#服务器集群)
    - [网络服务分层](network/layers)
    - [并发](network/concurrency)
* **框架**
    - [网络](network/layers)
    - [模块管理](src/doc/nel/net/module.md)
    - [服务主循环](src/doc/nel/service/main.md)
* [**数据库**](database)
    - MySQL
    - Mirror 游戏镜像数据库    
* **配置文件**
    - [命令](cfg/commands)
* **部署**
    - [php解难](deployment/troubleshooting/php)
* [**序列化**](src/doc/serialization)
* **协议**
    
