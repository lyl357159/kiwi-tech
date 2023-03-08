# gRPC
  - gRPC是由 google开发的一个高性能、通用的开源RPC框架。主要面向移动应用开发且基于HTTP/2协议标准而设计，同时支持大多数流行的编程语言。
  - HTTP/2相比HTTP1.x，有以下一些优势：
    - **用于数据传输的二进制分帧** HTTP/2采用二进制格式传输协议，而非HTTP/1.x的文本格式。
    - **多路复用** HTTP/2支持通过同一个连接发送多个并发的请求。而HTTP/1.x虽然通过pipeline也能并发请求，但多个请求之间的响应依然会被阻塞。
    - **服务端推送** HTTP/2中，服务器可以对客户端的一个请求发送多个响应。而不像HTTP/1.X一样，只能通过客户端发起request,服务端才产生对应的response
    - **减少网络流量的头部压缩**:HTTP/2对消息头进行了压缩传输，能够节省消息头占用的网络流量。至于如何压缩的，可以查看这篇：HPACK: Header Compression for HTTP/2[1]同时gRPC使用Protocol Buffers作为序列化协议

## HTTP 和 RPC 有什么区别
  - 首先这个问题本身不太严谨。HTTP只是一个通信协议，工作在OSI第七层。而RPC是一个完整的远程调用方案。它包含了:**接口规范、传输协议、数据序列化反序列化规范**。这样看，RPC和 HTTP的关系只可能是包含关系。为什么是可能？因为RPC传输协议那块我可以不基于HTTP。
  
## gRPC包含以下几个部分
   - Protocol Buffers：gRPC使用 Protocol Buffers 作为其接口定义语言（IDL），用于定义服务接口和消息格式。

   - gRPC API：基于Protocol Buffers生成的服务端和客户端的 API 接口，用于定义和调用服务方法。

   - gRPC Runtime：提供了gRPC客户端和服务端的底层实现，包括基于HTTP/2协议的双向流式数据传输、负载均衡、故障转移、认证和授权等功能。

   - gRPC 工具集：提供了一系列工具，包括gRPC命令行工具、代码生成器、调试工具等，用于方便地生成和使用gRPC服务。
   - gRPC 生态系统：gRPC生态系统中包含了大量支持gRPC的第三方库和框架，用于构建分布式应用程序，如Kubernetes、Istio等。

## 参考资料
  - [什么是 gRPC](https://blog.csdn.net/kevin_tech/article/details/120681720)


---
  - [返回首页](../../../README.md)