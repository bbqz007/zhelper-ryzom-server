使用HSHA并发模式。

[服务主循环](../../src/doc/nel/service/main.md)

只有[主线程](../../src/doc/nel/service/main.md)update时分派所有L3连接的消息，每个L3连接有独立的io线程池。所有io线程属于HA层，[主线程](../../src/doc/nel/service/main.md)提供HS服务，第个L3连接有一个队列属于队列层。被动端口使用io线程池，每个线程负责一定数量的io复用，每个连接按策略被分配到一个io线程进行多路复用。

frontend服务使用udp，不必复用io，所以只使用一个单独的io线程，队列层使用了双队列缓冲，HA与HS层分离临界资源访问。并且主线程只在收到tick服务的TOCK才会去分派udp端口上的消息。

由于只有[主线程](../../src/doc/nel/service/main.md)分派消息以及事件，所以基本上不使用锁，除了IO线程的消息队列是临界资源。最大程度地避免了同步锁操作，这与ZMQ的作者的理念一样。并使所有消息及事件按顺序分派。
