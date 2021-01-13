## overview
总共有两层网络，L5与L6.

L5依赖NS，服务用名字注册到NS，由NS分配端口，广播端口信息，L5网络服务自动维护所有使用NS的服务间的连接，使用者无须关心远端服务位置信息。

rws,fes,egs,ais,gpms,ios,tick,mirror组成一个服务网络。

L6模块gateway网络，每个服务进程可以有任意个gateway，加入不同的模块网络。典型地每个服务都创建一个aes_gw，连接到AES，这个gateway网络会将modulemessage路由转发到目的模块。

### listen ports
```
tcp        0      0 0.0.0.0:41292           0.0.0.0:*               LISTEN      13466/./logger_serv 
tcp        0      0 0.0.0.0:46700           0.0.0.0:*               LISTEN      13477/./ryzom_admin 
tcp        0      0 0.0.0.0:46701           0.0.0.0:*               LISTEN      13477/./ryzom_admin 
tcp        0      0 0.0.0.0:46702           0.0.0.0:*               LISTEN      13217/./ryzom_admin 
tcp        0      0 0.0.0.0:49950           0.0.0.0:*               LISTEN      13228/./backup_serv 
tcp        0      0 0.0.0.0:49970           0.0.0.0:*               LISTEN      13228/./backup_serv 
tcp        0      0 0.0.0.0:49980           0.0.0.0:*               LISTEN      13381/./mail_forum_ 
tcp        0      0 0.0.0.0:49990           0.0.0.0:*               LISTEN      13228/./backup_serv 
tcp        0      0 0.0.0.0:50000           0.0.0.0:*               LISTEN      13268/./ryzom_namin 
tcp        0      0 0.0.0.0:50503           0.0.0.0:*               LISTEN      13410/./shard_unifi 
tcp        0      0 0.0.0.0:50505           0.0.0.0:*               LISTEN      13410/./shard_unifi 
tcp        0      0 0.0.0.0:51000           0.0.0.0:*               LISTEN      13251/./gpm_service 
tcp        0      0 0.0.0.0:51001           0.0.0.0:*               LISTEN      13284/./ryzom_welco 
tcp        0      0 0.0.0.0:51002           0.0.0.0:*               LISTEN      13260/./input_outpu 
tcp        0      0 0.0.0.0:51003           0.0.0.0:*               LISTEN      13238/./entities_ga 
tcp        0      0 0.0.0.0:51004           0.0.0.0:*               LISTEN      13304/./tick_servic 
tcp        0      0 0.0.0.0:51005           0.0.0.0:*               LISTEN      13326/./mirror_serv 
tcp        0      0 0.0.0.0:51006           0.0.0.0:*               LISTEN      13356/./ai_service  
tcp        0      0 0.0.0.0:51007           0.0.0.0:*               LISTEN      13433/./frontend_se 
tcp        0      0 0.0.0.0:51008           0.0.0.0:*               LISTEN      13898/./session_bro 
tcp        0      0 0.0.0.0:irdmi           0.0.0.0:*               LISTEN      31492/nginx: master 
tcp        0      0 0.0.0.0:mysql           0.0.0.0:*               LISTEN      31947/mysqld        
tcp6       0      0 [::]:igrid              [::]:*                  LISTEN      24711/php-fpm: mast 
tcp        0      0 localhost:cslistener    0.0.0.0:*               LISTEN      12995/php-fpm: mast 
```
### NS
who connected to NS (there is a network containing 9\*8/2 <just like the sum of sequence\[1,9]> connections)
```
0	localhost:41381		localhost:50000		13238/./entities_ga
0	localhost:41373		localhost:50000		13251/./gpm_service
0	localhost:41379		localhost:50000		13260/./input_outpu
0	localhost:41375		localhost:50000		13284/./ryzom_welco
0	localhost:41383		localhost:50000		13304/./tick_servic
0	localhost:41395		localhost:50000		13326/./mirror_serv
0	localhost:41409		localhost:50000		13356/./ai_service
0	localhost:41434		localhost:50000		13433/./frontend_se
0	localhost:41473		localhost:50000		13898/./session_bro
```
### SU
who connected to SU
```
0	localhost:39504		localhost:50505		13238/./entities_ga
0	localhost:39503		localhost:50505		13260/./input_outpu
0	localhost:39497		localhost:50505		13284/./ryzom_welco
```
### GSU
who connected to Global SU
```
0	localhost:41458		localhost:50503		13238/./entities_ga
0	localhost:41447		localhost:50503		13260/./input_outpu
```
### AES
who connected to AES
```
0	localhost:53541		localhost:46702		13217/./ryzom_admin
0	localhost:53539		localhost:46702		13228/./backup_serv
0	localhost:53575		localhost:46702		13238/./entities_ga
0	localhost:53558		localhost:46702		13251/./gpm_service
0	localhost:53589		localhost:46702		13260/./input_outpu
0	localhost:53548		localhost:46702		13268/./ryzom_namin
0	localhost:53553		localhost:46702		13284/./ryzom_welco
0	localhost:53569		localhost:46702		13304/./tick_servic
0	localhost:53584		localhost:46702		13326/./mirror_serv
0	localhost:53601		localhost:46702		13356/./ai_service
0	localhost:53602		localhost:46702		13381/./mail_forum_
0	localhost:53609		localhost:46702		13410/./shard_unifi
0	localhost:53620		localhost:46702		13433/./frontend_se
0	localhost:53626		localhost:46702		13466/./logger_serv
0	localhost:53631		localhost:46702		13477/./ryzom_admin
0	localhost:53659		localhost:46702		13898/./session_bro
```
### AS
who connected to AS(ras)
```
0	localhost:36384		localhost:46701		13217/./ryzom_admin
```
### BMS
who use backup service
```
0	localhost:44688		localhost:49950		13238/./entities_ga
0	localhost:44685		localhost:49950		13304/./tick_servic
0	localhost:44764		localhost:49950		13356/./ai_service
0	localhost:50552		localhost:49990		13238/./entities_ga
0	localhost:50546		localhost:49990		13260/./input_outpu
0	localhost:50550		localhost:49990		13304/./tick_servic
0	localhost:50584		localhost:49990		13356/./ai_service
0	localhost:50598		localhost:49990		13410/./shard_unifi
0	localhost:50615		localhost:49990		13466/./logger_serv
0	localhost:50640		localhost:49990		13898/./session_bro
```
### MFS
who use mail forum service
```
0	localhost:53027		localhost:49980		13238/./entities_ga
```
