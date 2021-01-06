以
```
StartCommands=
{
	// Create a gateway module
	"moduleManager.createModule StandardGateway gw",
	// add a layer 5 transport
	"gw.transportAdd L5Transport l5",
	// open the transport
	"gw.transportCmd l5(open)",

	/// Create default connection with admin executor service
	// Create a gateway module
	"moduleManager.createModule StandardGateway gw_aes",
	// create the admin executor service module
	"moduleManager.createModule AdminExecutorServiceClient aes_client",
	"aes_client.plug gw_aes",

	// create a layer 3 client to connect to aes gateway
	"gw_aes.transportAdd L3Client aes_l3c",
	"gw_aes.transportCmd aes_l3c(connect addr="+AESHost+":"+AESPort+")",

	// create the admin executor service module
	"moduleManager.createModule AdminExecutorService aes",

	// create a gateway to connect to as
	"moduleManager.createModule StandardGateway asc_gw",
	// create a layer 3 client
	"asc_gw.transportAdd L3Client l3c",
	"asc_gw.transportCmd l3c(connect addr="+ASHost+":"+ASPort+")",

	// create a gateway for services to connect
	"moduleManager.createModule StandardGateway aes_gw",
	// create a layer 3 server
	"aes_gw.transportAdd L3Server l3s",
	"aes_gw.transportOptions l3s(PeerInvisible)",
	"aes_gw.transportCmd l3s(open port="+AESPort+")",

	// plug the as
	"aes.plug asc_gw",
	"aes.plug aes_gw",
};
```
```moduleManager.createModule StandardGateway gw```创建gateway模块，变量名gw。
```gw.transportAdd L5Transport l5```创建L5对象，变量名l5
```gw.transportCmd l5(open)```执行l5.open()
```asc_gw.transportAdd L3Client l3c```创建CCallbackClient，变量名l3c
```asc_gw.transportCmd l3c(connect addr="+ASHost+":"+ASPort+")",```执行l3c.connect()
```aes_gw.transportAdd L3Server l3s```创建CCallbackServer
```aes_gw.transportOptions l3s(PeerInvisible)```属性设置
```aes_gw.transportCmd l3s(open port="+AESPort+")```listen端口
