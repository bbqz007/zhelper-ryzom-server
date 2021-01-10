## CSock
```c++
/**
 * CSock: base socket class.
 * One CSock object represents a communication between two hosts, the local one and the remote one.
 * This class implements layer 0 of the NeL Network Engine.
 * This class does not handle conversion between big endian and little endian ; the provided
 * buffers are sent raw.
 *
 * The "logging" boolean value is necessary because in this implementation we always log
 * to one single global CLog object : there is not one CLog object per socket. Therefore
 * we must prevent the socket used in CNetDisplayer from logging itself... otherwise we
 * would have an infinite recursion.
 *
 * The "connected" property may have a different meaning whether the socket is a stream socket
 * (e.g. using TCP) or it is a connectionless datagram socket (e.g. using UDP). In the latter,
 * "connected" only means that the local and the remote addresses have been set.
 *
 * Important note: this class is thread-safe, meaning you can access to a CSock object
 * from multiple threads BUT the only things you are allow to do in parallel are
 * receive/send and read the connected() property.
 *
 * You must call CSock::initNetwork() before using any network class (even CInetAddress).
 * You must call CSock::releaseNetwork() at the end of your program.
 *
 * By default, a socket is in blocking mode. Call setNonBlockingMode() to change this
 * behaviour.
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2000-2001
 */
class CSock
```

## CTcpSock
```c++
/**
 * CTcpSock: Reliable socket via TCP.
 * See base class CSock.
 *
 * When to set No Delay mode on ?
 * Set TCP_NODELAY (call setNoDelay(true)) *only* if you have to send small buffers that need to
 * be sent *immediately*. It should only be set for applications that send frequent small bursts
 * of information without getting an immediate response, where timely delivery of data is
 * required (the canonical example is mouse movements). Setting TCP_NODELAY on increases
 * the network traffic (more overhead).
 * In the normal behavior of CSock, TCP_NODELAY is off i.e. the Nagle buffering algorithm is enabled.
 *
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2000-2001
 */
class CTcpSock : public CSock
```


## CUdpSock
```c++
/**
 * CUdpSock: Unreliable datagram socket via UDP.
 * See base class CSock.
 * \author Olivier Cado
 * \author Nevrax France
 * \date 2000-2001
 */
class CUdpSock : public CSock
```
