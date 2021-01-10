## CNetManager
```c++
/**
 * Layer 4
 *
 * In case of addGroup(), messages are *not* associate with id, so the message type is always sent with string.
 *
 * \author Vianney Lecroart
 * \author Nevrax France
 * \date 2001
 */
class CNetManager
{
public:

	/** Creates the connection to the Naming Service.
	 * If the connection failed, ESocketConnectionFailed exception is generated.
	 */
	static void	init (const CInetAddress *addr, CCallbackNetBase::TRecordingState rec );

	static void release ();

	/** Sets up a server on a specific port with a specific service name (create a listen socket, register to naming service and so on)
	 * If servicePort is 0, it will be dynamically determinated by the Naming Service.
	 * If sid id 0, the service id will be dynamically determinated by the Naming Service.
	 */
	static void	addServer (const std::string &serviceName, uint16 servicePort = 0, bool external = false);

	static void	addServer (const std::string &serviceName, uint16 servicePort, NLNET::TServiceId &sid, bool external = false);

	/// Creates a connection to a specific IP and associate it this a "fake" serviceName (to enable you to send data for example)
	static void	addClient (const std::string &serviceName, const std::string &addr, bool autoRetry = true);

	/// Creates a connection to a service using the naming service and the serviceName
	static void	addClient (const std::string &serviceName);

	/// Creates connections to a group of service
	static void	addGroup (const std::string &groupName, const std::string &serviceName);

	/// Adds a callback array to a specific service connection. You can add callback only *after* adding the server, the client or the group
	static void	addCallbackArray (const std::string &serviceName, const TCallbackItem *callbackarray, NLMISC::CStringIdArray::TStringId arraysize);

	/** Call it evenly. the parameter select the timeout value in milliseconds for each update. You are absolutely certain that this
	 * function will not be returns before this amount of time you set.
	 * If you set the update timeout value higher than 0, all messages in queues will be process until the time is greater than the timeout user update().
	 * If you set the update timeout value to 0, all messages in queues will be process one time before calling the user update(). In this case, we don't nlSleep(1).
	 */
	static void	update (NLMISC::TTime timeout = 0);

	/// Sends a message to a specific serviceName
	static void	send (const std::string &serviceName, const CMessage &buffer, TSockId hostid = InvalidSockId);
```
