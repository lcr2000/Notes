##### http包执行流程

1.创建Listen Socket，监听指定的端口，等待客户端请求到来。

2.Listen Socket接受客户端的请求，得到Client Socket，接下来通过Client Socket与客户端通信。

3.处理客户端的请求，首先从Client Socket读取HTTP请求的协议头，如果是POST方法，还可能要读取客户端提交的数据，然后交给相应的handler处理请求，handler处理完毕准备好客户端需要的数据，通过Client Socket写给客户端。