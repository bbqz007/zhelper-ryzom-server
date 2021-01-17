[doc license](https://github.com/bbqz007/zhelper-ryzom-server/LICENSE)

## OVERVIEW

RyzomClient与Frontend之间以Impulsion消息的方式传递数据。一个UDP包只包含一个Impulsion消息

RyzomClient与游戏服务交换的消息，以CAction为单位。一个Impulsion消息可以携带一个或多个CAction。或者一个CAction需要分解成多个Blocks，由多个Impulsion消息完成传递。

RyzomClient不能与任何游戏服务直接交换消息，只能与Frontend通信。

Frontend不能将Impulsion消息直接发送到任何游戏服务，必须通过Mirror转发，并在协议加上前缀**"Client:"**。

任何游戏服务依赖Frontend的cbImpulsionXXX协议服务，将Impulsion消息经由Frontend向RyzomClient发送。

