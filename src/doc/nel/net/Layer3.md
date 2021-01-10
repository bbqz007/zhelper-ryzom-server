## CCallbackNetBase
```c++
class CCallbackNetBase;

/** Callback function type for message processing
 *
 * msgin contains parameters of the message
 * from is the SockId of the connection, for a client, from is always the same value
 */
typedef void (*TMsgCallback) (CMessage &msgin, TSockId from, CCallbackNetBase &netbase);


/// Callback items. See CMsgSocket::update() for an explanation on how the callbacks are called.
typedef struct
{
	/// Key C string. It is a message type name, or "C" for connection or "D" for disconnection
	const char		*Key;
	/// The callback function
	TMsgCallback	Callback;

} TCallbackItem;


/**
 * Layer 3
 * \author Vianney Lecroart
 * \author Nevrax France
 * \date 2001
 */
class CCallbackNetBase
{
	// contains callbacks
	std::vector<TCallbackItem>	_CallbackArray;

	// called if the received message is not found in the callback array
	TMsgCallback				_DefaultCallback;

	// If not null, called before each message is dispached to it's callback
	TMsgCallback				_PreDispatchCallback;
};
```

## CCallbackClient
```c++
/**
 * Client class for layer 3
 * \author Vianney Lecroart, Olivier Cado
 * \author Nevrax France
 * \date 2001
 */
class CCallbackClient : public CCallbackNetBase, public CBufClient
{
```

## CCallbackServer
```c++
/**
 * Server class for layer 3
 * \author Vianney Lecroart
 * \author Nevrax France
 * \date 2001
 */
class CCallbackServer : public CCallbackNetBase, public CBufServer
{
```
