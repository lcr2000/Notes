RPC的作用主要有三个方面：进程间通讯、提供和本地方法调用一样的调用机制、屏蔽程序员对远程调用的细节实现[



## gRPC概述

gRPC](http://www.grpc.io/)是一个高性能、通用的开源 RPC 框架，其由 Google 主要面向移动应用开发并基于[HTTP/2](https://http2.github.io/)协议标准而设计，基于[ProtoBuf](http://en.wikipedia.org/wiki/Protocol_Buffers)(Protocol Buffers) 序列化协议开发，且支持众多开发语言。gRPC 提供了一种简单的方法来精确地定义服务和为 iOS、Android 和后台支持服务自动生成可靠性很强的客户端功能库。客户端充分利用高级流和链接功能，从而有助于节省带宽、降低的 TCP 链接次数、节省 CPU 使用、和电池寿命



## gRPC具有以下特征

* 强大的 IDL 特性(interface description language)

* 基于ProtoBuf(Protocol Buffers)定义接口规范

ProtoBuf 是由 Google 开发的一种数据序列化协议（类似于 XML、JSON、hessian）。ProtoBuf 能够将数据进行序列化，并广泛应用在数据存储、通信协议等方面。不过，当前 gRPC 仅支持 Protobuf ，且不支持在浏览器中使用。由于 gRPC 的设计能够支持支持多种数据格式，所以读者能够很容易实现对其他数据格式（如 XML、JSON 等）的支持。

### 为什么使用ProtoBuf？

适合应用场景：它适合做数据存储或RPC数据交换格式。可用于通讯协议、数据存储等领域的语言无关、平台无关、可扩展的序列化数据结构。





