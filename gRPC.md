RPC的作用主要有三个方面：进程间通讯、提供和本地方法调用一样的调用机制、屏蔽程序员对远程调用的细节实现[



## gRPC概述

[gRPC](http://www.grpc.io/)是一个高性能、通用的开源 RPC 框架，其由 Google 主要面向移动应用开发并基于[HTTP/2](https://http2.github.io/)协议标准而设计，基于[ProtoBuf](http://en.wikipedia.org/wiki/Protocol_Buffers)(Protocol Buffers) 序列化协议开发，且支持众多开发语言。gRPC 提供了一种简单的方法来精确地定义服务和为 iOS、Android 和后台支持服务自动生成可靠性很强的客户端功能库。客户端充分利用高级流和链接功能，从而有助于节省带宽、降低的 TCP 链接次数、节省 CPU 使用、和电池寿命



## gRPC具有以下特征

- 强大的 IDL 特性(interface description language)

基于ProtoBuf(Protocol Buffers)定义服务（定义接口规范）

ProtoBuf 是由 Google 开发的一种数据序列化协议（类似于 XML、JSON、hessian）。ProtoBuf 能够将数据进行序列化，并广泛应用在数据存储、通信协议等方面。不过，当前 gRPC 仅支持 Protobuf ，且不支持在浏览器中使用。由于 gRPC 的设计能够支持支持多种数据格式，所以读者能够很容易实现对其他数据格式（如 XML、JSON 等）的支持。

为什么使用ProtoBuf？

适合应用场景：它适合做数据存储或RPC数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化数据结构。

以下是ProtoBuf的语法例子：

```
message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
```

定义服务的示例代码如下：

```
message HelloRequest {
  string greeting = 1;
}
message HelloResponse {
  string reply = 1;
}
service HelloService {
  rpc SayHello(HelloRequest) returns(HelloResponse);
}
```

- 支持多种语言

gRPC 支持多种语言，并能够基于语言自动生成客户端和服务端功能库。目前，在 GitHub 上已提供了 C 版本[grpc](https://github.com/grpc/grpc)、Java 版本[grpc-java](https://github.com/grpc/grpc-java) 和 Go 版本[grpc-go](https://github.com/grpc/grpc-go)，其它语言的版本正在积极开发中，其中 grpc 支持 C、[C++](https://github.com/grpc/grpc/tree/master/src/cpp)、[Node.js](https://github.com/grpc/grpc/tree/master/src/node)、[Python](https://github.com/grpc/grpc/tree/master/src/python)、[Ruby](https://github.com/grpc/grpc/tree/master/src/ruby)、[Objective-C](https://github.com/grpc/grpc/tree/master/src/objective-c)、[PHP](https://github.com/grpc/grpc/tree/master/src/php)和[C#](https://github.com/grpc/grpc/tree/master/src/csharp)等语言，grpc-java 已经支持 Android 开发

- 基于HTTP/2标准设计

由于 gRPC 基于 HTTP/2 标准设计，所以相对于其他 RPC 框架，gRPC 带来了更多强大功能，如双向流、头部压缩、多复用请求等。这些功能给移动设备带来重大益处，如节省带宽、降低 TCP 链接次数、节省 CPU 使用和延长电池寿命等。同时，gRPC 还能够提高了云端服务和 Web 应用的性能。gRPC 既能够在客户端应用，也能够在服务器端应用，从而以透明的方式实现客户端和服务器端的通信和简化通信系统的构建

## gRPC使用案例

Person的服务

1、通过ProtoBuf定义接口规范

- 定义消息体(message)

```
message Person {
  string name = 1;
  int32 age = 2;
}
message ResponseMessage {
  string message = 1;
}
message QueryPersonRequest {
  string name = 1;
}
```

- 定义服务接口(service)

2、gRPC服务端与客户端实现

- 简单的RPC调用（不使用流操作） A simple RPC

特点：服务器与客户端的数据交互量非常小

服务端实现

客户端实现

- 客户端到服务器端的单向流  A client to server  streaming RPC

特点：服务端返回大量数据到客户端，客户端上传非常小的数据量给服务端

服务器端实现

客户端实现

- 客户端与服务器端的双向流  A Bidirectional streaming RPC

适用场景：客户端上传大量数据到服务端，服务端返回给客户端的数据量也很大

服务端实现

客户端实现

- 服务端如何监听客户端的请求
- 客户端如何请求指定的服务端


## 生成gRPC代码

一旦定义好服务，我们可以使用protocol buffer 编译器 `protoc` 来生成创建应用所需的特定客户端和服务端的代码，可以生成任意 gRPC 支持的语言的代码。

为了生成客户端和服务端接口，运行 protocol buffer 编译器：

```
protoc -I ../protos ../protos/helloworld.proto --go_out=plugins=grpc：helloworld
```

这生成了 `helloworld.pb.go` ，包含了我们生成的客户端和服务端类，此外还有用于填充、序列化、提取 `HelloRequest` 和 `HelloResponse` 消息类型的类。

## 写一个服务器

创建一个服务应用来实现服务

### 服务实现

服务器有一个 `server` 结构。它通过实现 `sayHello` 方法，实现了从 proto 服务定义生成的`GreeterServer` 接口：

```
// server is used to implement helloworld.GreeterServer.
type server struct{}
// SayHello implements helloworld.GreeterServer
func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
  return &pb.HelloReply{Message： "Hello " + in.Name}, nil
}
```

为了返回给客户端应答并且完成调用：

1. 用我们的激动人心的消息构建并填充一个在我们接口定义的 `HelloReply` 应答对象。
2. 将 `HelloReply` 返回给客户端。

### 服务端实现

```
const (
  port = "：50051"
)
...
func main() {
  lis, err ：= net.Listen("tcp", port)
  if err != nil {
      log.Fatalf("failed to listen： %v", err)
  }
  s ：= grpc.NewServer()
  pb.RegisterGreeterServer(s, &server{})
  s.Serve(lis)
}
```

## 写一个客户端

客户端的 gRPC 非常简单。在这一步，用生成的代码写一个简单的客户程序来访问创建的 `Greeter` 服务器。

### 连接服务

需要创建一个 gRPC 频道，指定我们要连接的主机名和服务器端口。然后我们用这个频道创建存根实例。

```
const (
  address     = "localhost：50051"
  defaultName = "world"
)
func main() {
  // Set up a connection to the server.
  conn, err ：= grpc.Dial(address)
  if err != nil {
      log.Fatalf("did not connect： %v", err)
  }
  defer conn.Close()
  c ：= pb.NewGreeterClient(conn)
...
}
```

### 调用RPC

现在我们可以联系服务并获得一个 greeting ：

1. 我们创建并填充一个 `HelloRequest` 发送给服务。
2. 我们用请求调用存根的 `SayHello()`，如果 RPC 成功，会得到一个填充的 `HelloReply` ，从其中我们可以获得 greeting。

```
r, err ：= c.SayHello(context.Background(), &pb.HelloRequest{Name： name})
if err != nil {
      log.Fatalf("could not greet： %v", err)
}
log.Printf("Greeting： %s", r.Message)
```

## gRPC 的一个功能

不同的语言间的互操作性，即在不同的语言运行客户端和服务端。每个服务端和客户端使用从同一个 proto 文件生成的接口代码，则意味着任何 `Greeter` 客户端可以与任何 `Greeter` 服务端对话。

- 命令

```
$ greeter_server &
```




