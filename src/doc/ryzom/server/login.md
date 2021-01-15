WS, FES

WS提供CWelcomeServiceMod模块服务给PHP，GameClient用http选择SHARD登陆，在WS保留一个证明。

GameClient向游戏服务器的前端FES登入，FES向WS发送＂SCS＂协议，WS处理登陆证明，返回结果到PHP，PHP返回结果到GameClient。

GameClient收到PHP成功的结果后完成游戏登入，继续与游戏服务器的前端FES通信。
