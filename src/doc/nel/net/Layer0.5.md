## CBufSock
```c++
/**
 * CBufSock
 * A socket and its sending buffer
 */
class CBufSock
{
public:
protected:

	friend class CBufClient;
	friend class CBufServer;
	friend class CClientReceiveTask;
	friend class CServerReceiveTask;

	friend class CCallbackClient;
	friend class CCallbackServer;
	friend class CCallbackNetBase;
	/// Update the network sending (call this method evenly). Returns false if an error occured.
	bool	update();
  
	// Send queue
	NLMISC::CBufFIFO	SendFifo;

	// Socket (pointer because it can be allocated by an accept())
	CTcpSock			*Sock;
```

## CNonBlockingBufSock
```c++
/**
 * CNonBlockingBufSock
 * A socket, its send buffer plus a nonblocking receiving system
 */
class CNonBlockingBufSock : public CBufSock
{
protected:

	friend class CBufClient;
	friend class CClientReceiveTask;
  
  // Buffer for nonblocking receives
	std::vector<uint8>			_ReceiveBuffer;

	// Max payload size than can be received in a block
	uint32						_MaxExpectedBlockSize;
```

## CServerBufSock
```c++
/**
 * CServerBufSock
 * A socket, its send buffer plus a nonblocking receiving system for a server connection
 */
class CServerBufSock : public CNonBlockingBufSock
{
protected:

	friend class CBufServer;
	friend class CListenTask;
	friend class CServerReceiveTask;
```
