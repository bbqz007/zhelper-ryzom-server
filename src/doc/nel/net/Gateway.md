## overview
* module_gateway.cpp
* module_gateway_transport.cpp
* module_l5_transport.cpp

进程间或本地间的模块互连（interconnect），发送或分派模块消息（sendModuleMessage，dispatchModuleMessage）。

Transport就是Gateway依赖的传输层实现，可以是L3或L5，也可以是namingpipe。

一个Transport只能是主动端（client）或被动端（server）。

Route代表一个Transport上的连接，在这个连接上的名字。

Gateway必须是一个Module，并且是一个特殊的Module。

CModuleSocket是一个特殊的Module，并且是抽象的（有虚函数没实现）。

CStandardGateway即是一个Gateway，也是一个CModuleSocket。

CStandardGateway管理远端ModuleProxy，CModuleManager管理本地IModule。

[详细参看模块管理]()



```c++
    /** Interface for gateway.
	 *	A gateway is the part of the module layer that interconnect
	 *	module locally and give access to foreign module hosted in
	 *	other process.
	 *	Gateway can interconnect local module with themselves, as well as
	 *	connect with another gateway in another process or host.
	 *
	 *	Transport:
	 *	---------
	 *	Gateway connectivity is provided by 'transport' that can
	 *	be build on any support.
	 *	There are available transport for NeL Layer 3 in server
	 *	of client mode.
	 *
	 *	You can bind at run time, any number of transport to a
	 *	gateway.
	 *	Each of these transport can then receive specific command
	 *	from passed by the gateway.
	 *	Transport then create routes that are active connection
	 *	to foreign gateways.
	 *
	 *	When using the layer 3 transport, you can choose either
	 *	to instantiate a client mode transport or a server mode
	 *	transport.
	 *	In client mode, you can connect to one or more server, each
	 *	connection generating a new route.
	 *	In server mode, you can put your transport as 'open' on
	 *	a specified TCP port, then each client connection will
	 *	generate a new route.
	 *
	 *	These layer 3 transport are only one type of transport,
	 *	it it possible to build any type of transport (on raw TCP
	 *	socket, or over UDP, or using named pipe...).
	 *
	 *	Unique name generation :
	 *	------------------------
	 *	Gateway need to generate 'fully qualified module name' witch
	 *	must be unique in any gateway interconnection scheme.
	 *	By default, gateway use the following scheme to build
	 *	unique module name :
	 *		<hostname>:<pid>:<modulename>
	 *	In some case, you could not be sure that the host name will
	 *	be unique (think about your hundreds simultaneously connected
	 *	clients, some of them can have set the same name for their
	 *	computer !). In this case, you can set register a unique name
	 *	generator on you gateway that use another naming scheme.
	 *	For example, you can use you customer unique ID (with the
	 *	restriction that one ID can be in use once at the same time)
	 *	to build unique name :
	 *		<customerId>:<modulename>
	 *
	 *	Advanced transport options (i.e option settable on each transport):
	 *	--------------------------
	 *	- peer invisible : if activated on a transport, this option
	 *		will mask the modules of the other routes of the same
	 *		transport.
	 *		This is useful for players modules (you sure don't want
	 *		that all players can see all other clients modules),
	 *		or for client/server structure where you don't want
	 *		all client to see all module, bu only those that are
	 *		available from the client side.
	 *		Note that any module that come from another transport
	 *		will be disclosed to all the route.
	 *
	 *	- firewalled : if activated, this option will mask any module
	 *		to the connected unsecure route unless some module comming from another
	 *		transport (or a local module) will try to send a message
	 *		to a module on an unsecure route. In this case, the gateway
	 *		will disclose the information about this sender module (and only
	 *		this one) to the unsecure route.
	 *		In firewall mode, the gateway keep a list of disclosed module
	 *		for each route and stop any message that is addressed to an
	 *		undisclosed module (that can be considered as a hacking attempt).
	 *		Module that are disclosed by the unsecure routes are normaly
	 *		seen by all other module.
	 *
	 *	The two options above can be used in combination. This is a prefered
	 *	way of configuring transport for player connection : we don't want
	 *	player to see other player modules, and we don't want player to see
	 *	any server side module until one of them started a communication
	 *	with a player module.
	 */
	class IModuleGateway : public NLMISC::CRefCount
```
```c++
    /** Intermediate class must be used as base class
	 *	for implementing gateway.
	 */
	class CModuleGateway : public IModuleGateway
```
```c++
    /** Interface class for gateway transport.
	 *	A gateway transport is an object associated to a standard gateway
	 *	at run time and that provide a mean to interconnect with
	 *	other gateway.
	 *	As each transport mode as it's own command requirement,
	 *	a generic command system is provided for sending command message
	 *	to the transport implementation.
	 *
	 *	At time of writing, NeL come with 2 transport : one based on layer 3 client, and one
	 *	based on layer 3 server. In a short time, there will be transport using layer 5.
	 */
	class IGatewayTransport
```
```c++
	/** Base class for gateway route.
	 *	Route are provided by transport.
	 *	Transport provide a mean to build route
	 *	between gateway.
	 *	Route show the list of foreign gateway that are
	 *	reachable with it and are use to send
	 *	message to these gateways.
	 *
	 *	The route store proxy id translation table, i.e,
	 *	for each module proxy that come from this route
	 *	we store association of the local proxy ID with
	 *	the foreign proxy ID, that is the proxy that
	 *	represent the module at the outbound of the route.
	 *
	 *	Note that even if the route object is created
	 *	by the transport, the translation table is
	 *	feed and managed by the gateway implementation.
	 */
	class CGatewayRoute
```

## CStandardGateway
```c++
	/** The standard gateway that interconnect module
	 *	across process.
	 */
	class CStandardGateway :
		public CModuleBase,
		public CModuleGateway,
		public CModuleSocket
```
## L3Transport
```c++
	/** the specialized route for server transport */
	class CL3ServerRoute : public CGatewayRoute
	{
	public:
		/// The id of the socket in the server
		TSockId			SockId;

		/// Time stamp of last message received/emitted
		mutable uint32	LastCommTime;
```
```c++
	/** Gateway transport using layer 3 server */
	class CGatewayL3ServerTransport : public IGatewayTransport
	{
		friend class CL3ServerRoute;
	public:
		/// The callback server that receive connection and dispatch message
		auto_ptr<CCallbackServer>			_CallbackServer;
```
```c++
	class CL3ClientRoute : public CGatewayRoute
	{
	public:
		/// The server address for this route
		CInetAddress				ServerAddr;
		/// The Client callback
		mutable CCallbackClient		CallbackClient;
```
```c++
	/** Gateway transport using layer 3 client */
	class CGatewayL3ClientTransport : public IGatewayTransport
	{
		friend class CL3ClientRoute;
	public:
		/// A static mapper to retrieve transport from the CCallbackServer pointer
		typedef map<CCallbackNetBase*, CGatewayL3ClientTransport*>	TDispatcherIndex;
		static TDispatcherIndex				_DispatcherIndex;

		/// Storage for active connection
		typedef map<TSockId, CL3ClientRoute*>	TClientRoutes;
		TClientRoutes			_Routes;

		/// Indexed storage of active connection (used for stable connId)
		/// a NULL TSockeId mean a free connection slot.
		typedef vector<TSockId>		TClientRouteIds;
		TClientRouteIds			_RouteIds;
		/// A list of free slot ready for use
		typedef vector<TClientRouteIds::difference_type>	TFreeRouteIds;
		TFreeRouteIds			_FreeRoutesIds;

		/// the route to delete outside of the update loop
		list<CL3ClientRoute*>	_RouteToRemove;
```

## L5Transport
```c++
	/** the specialized route for l5 transport */
	class CL5Route : public CGatewayRoute
	{
	public:
		/// the service ID of the outbound service
		TServiceId		ServiceId;

		/// The transport ID at the outbound
		TL5TransportId	ForeignTransportId;
```
```c++
	/** Gateway transport using layer 5 */
	class CGatewayL5Transport : public IGatewayTransport
	{
		friend class CL5Route;
	public:
		/// This transport ID
		TL5TransportId	_TransportId;

		/// current open status
		bool			_Open;
		/// Subnet name
		string			_SubNetName;

		typedef std::map<TServiceId, CL5Route*>	TRouteMap;
		/// The table that keep track of all routes
		TRouteMap	_Routes;
```
