# Thrift
 - Thrift 是一个软件框架（远程过程调用框架），用来进行可扩展且跨语言的服务的开发。它结合了功能强大的软件堆栈和代码生成引 擎，以构建在 C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk, and OCaml 这些编程语言间无缝结合的、高效的服务。
 - Thrift是一种接口描述语言和二进制通讯协议，它被用来定义和创建跨语言的服务。它被当作一个远程过程调用（RPC）框架来使用，是由Facebook为“大规模跨语言服务开发”而开发的。07 年四月开放源码，08 年 5 月进入 apache 孵化器，现在是 Apache 基金会的顶级项目。

## Thrift一些已经明确的优点
   - 跟一些替代选择，比如SOAP相比，跨语言序列化的代价更低，因为它使用二进制格式。
   - 它有一个又瘦又干净的库，没有编码框架，没有XML配置文件。
   - 绑定感觉很自然。例如，Java使用java.util.ArrayList<String>；C++使用std::vector<std::string>。
   - 应用层通讯格式与序列化层通讯格式是完全分离的。它们都可以独立修改。
   - 预定义的序列化格式包括：二进制格式、对HTTP友好的格式，以及紧凑的二进制格式。
   - 兼作跨语言文件序列化。
   - 协议使用软版本号机制软件版本管理。Thrift不要求一个中心化的和显式的版本号机制，例如主版本号/次版本号。松耦合的团队可以轻松地控制RPC调用的演进。
   - 没有构建依赖也不含非标准化的软件。不存在不兼容的软件许可证混用的情况。
---
## 参考资料
  - [Thrift 服务开发框架](https://www.oschina.net/p/thrift?hmsr=aladdin1e1)
---
- [返回首页](../../../README.md)