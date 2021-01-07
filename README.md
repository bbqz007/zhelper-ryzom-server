# zhelper-ryzom-server
ryzomcore(open 3d mmorpg game)服务端zhelper版帮助文档。

### 历史
dev.ryzom.com似乎在2012年后停止了，现在访问wiki.ryzom.dev。

代码仓地址：github.com/ryzom/ryzomcore

默认分支：develop （部分代码不完整，在[issue#587](https://github.com/ryzom/ryzomcore/issues/587)可以查到）

选用分支：classic-develop

### 服务器集群
包括16个服务进程，web服务（nginx+php-fpm），mysql（或mariadb）数据库。

* **AS** - ryzom_admin_service - 后台管理web端接口服务

* **bms_master** - backup_service 

* **egs** - entities_game_service - 主逻辑，规则判决

* **gpms** - gpm_service - 地理服务

* **ios** - input_output_service - 聊天，文本信息等

* **rns** - ryzom_naming_service - 后端名字服务

* **rws** - ryzom_welcome_service - 线路服务

* **ts** - tick_service - 世界时间

* **ms** - mirror_service - 类redis

* **ais_newbyland** - ai_service - NPC，ai

* **mfs** - mail_forum_service 

* **su** - shard_unifier_service 

* **fes** - frontend_service - 游戏端gateway

* **sbs** - session_browser_server 

* **lgs** - logger_service - 日志服务

* **ras** - ryzom_admin_service 后台管理控制台

