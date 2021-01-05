# zhelper-ryzom-server
ryzomcore(open 3d mmorpg game)服务端zhelper版帮助文档。

### 历史
dev.ryzom.com似乎在2012年后停止了，现在访问wiki.ryzom.dev。

代码仓地址：github.com/ryzom/ryzomcore

默认分支：develop （部分代码不完整，在issue可以查到）

选用分支：classic-develop

### 服务器集群
包括16个服务进程，web服务（nginx+php-fpm），mysql（或mariadb）数据库。

* **AS** ryzom_admin_service

* bms_master backup_service 

* egs entities_game_service 

* gpms gpm_service 

* ios input_output_service 

* rns ryzom_naming_service 

* rws ryzom_welcome_service 

* ts tick_service 

* ms mirror_service 

* ais_newbyland ai_service 

* mfs mail_forum_service 

* su shard_unifier_service 

* fes frontend_service 

* sbs session_browser_server 

* lgs logger_service

* ras ryzom_admin_service

