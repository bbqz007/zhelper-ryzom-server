## CBufNetBase
```c++
/**
 * Layer 1
 *
 * Base class for CBufClient and CBufServer.
 * The max block sizes for sending and receiving are controlled by setMaxSentBlockSize()
 * and setMaxExpectedBlockSize(). Their default value is the maximum number contained in a sint32,
 * that is 2^31-1 (i.e. 0x7FFFFFFF). The limit for sending is checked only in debug mode.
 *
 * \author Nevrax France
 * \date 2001
 */
class CBufNetBase
```

## CBufClient
```c++
/**
 * Client class for layer 1
 *
 * Active connection with packet scheme and buffering.
 * The provided buffers are sent raw (no endianness conversion).
 * By default, the size time trigger is disabled, the time trigger is set to 20 ms.
 *
 * Where do the methods take place:
 * \code
 * send()             ->  send buffer   ->  update(), flush(),
 *                                          bytesUploaded(), newBytesUploaded()
 *
 * receive(),         <- receive buffer <-  receive thread,
 * dataAvailable(),                         bytesDownloaded(), newBytesDownloaded()
 * disconnection callback
 * \endcode
 *
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2001
 */
class CBufClient : public CBufNetBase
{
protected:

	friend class CClientReceiveTask;

	/// Send buffer and connection
	CNonBlockingBufSock *_BufSock; // ADDED: non-blocking client connection

private:

	/// Receive task
	CClientReceiveTask	*_RecvTask;

	/// Receive thread
	NLMISC::IThread		*_RecvThread;
};
```
```c++
/**
 * Code of receiving thread for clients
 */
class CClientReceiveTask : public NLMISC::IRunnable
{
private:

	CBufClient					*_Client;

	CNonBlockingBufSock			*_NBBufSock; // CHANGED: non-blocking client connection
}
```

## 
```c++
/**
 * Server class for layer 1
 *
 * Listening socket and accepted connections, with packet scheme.
 * The provided buffers are sent raw (no endianness conversion).
 * By default, the size time trigger is disabled, the time trigger is set to 20 ms.
 *
 * Where do the methods take place:
 * \code
 * send(),	                           ->  send buffer   ->  update(), flush()
 * bytesSent(), newBytesSent()
 *
 * receive(), dataAvailable(),         <- receive buffer <-  receive thread,
 * dataAvailable(),
 * bytesReceived(), newBytesReceived(),
 * connection callback, disconnection callback
 * \endcode
 *
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2001
 */
class CBufServer : public CBufNetBase
{
	/// Listen task
	CListenTask						*_ListenTask;

	/// Listen thread
	NLMISC::IThread					*_ListenThread;

	/* Vector of receiving threads.
	 * Thread: thread control
	 * Thread->Runnable: access to the CServerReceiveTask object
	 * Thread->getRunnable()->sock(): access to the socket
	 * The purpose of this list is to delete the objects after use.
	 */
	NLMISC::CSynchronized<CThreadPool>		_ThreadPool;
};
```
```c++
/**
 * Code of receiving threads for servers.
 * Note: the methods locations in the classes do not correspond to the threads where they are
 * executed, but to the data they use.
 */
class CServerReceiveTask : public NLMISC::IRunnable, public CServerTask
{
private:
	CBufServer								*_Server;

	/* List of sockets and send buffer.
	 * A TSockId is a pointer to a CBufSock object
	 */
	NLMISC::CSynchronized<CConnections>		_Connections;

	// Connections to remove
	NLMISC::CSynchronized<CConnections>		_RemoveSet;
};
```
```c++
/**
 * Code of listening thread
 */
class CListenTask : public NLMISC::IRunnable, public CServerTask
{
```
