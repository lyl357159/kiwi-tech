# XSS
  - **跨站脚本攻击**
  - xss的产生原因就是对用户的输入过滤不严格，在能够同用户实现交互的地方更容易发现xss漏洞。
  - XSS攻击可以分为三类：反射型，存储型，DOM型
  
## 防御办法
  - 对输入进行检查和转码
    > 输入检查一般是检查用户输入的数据是都包含一些特殊字符，如 <、>, '及"等。如果发现特殊字符，则将这些字符过滤或编码。这种可以称为 “XSS Filter”。
  - 利用CSP
    > CSP (Content Security Policy) 即内容安全策略，是一种可信白名单机制，可以在服务端配置浏览器哪些外部资源可以加载和执行。可以通过这种方式来尽量减少 XSS 攻击。


## 参考资料
   - [XSS和CSRF两种攻击方式总结](https://blog.csdn.net/JunDao73/article/details/122897785)
   - [详解XSS和CSRF](https://www.cnblogs.com/zhouyyBlog/p/14505961.html)