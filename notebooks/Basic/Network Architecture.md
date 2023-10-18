# Nginx Web 服务器

Nginx(engine ）是一个高性能 HTTP 、反向代理、 IMAP POP3 SMTP 务器。

Nginx 相对于 Apache 优点如下：

- 高并发响应性能非常好，官方 Nginx 处理静态文件并发 /s

- 负载均衡及反向代理性能非常强；

- 系统内存和 CPU 占用率低；

- 可对后端服务进行健康检查

- 支持 PHP CGI 方式和 FastCGI 方式

- 可以作为缓存服务器、邮件代理服务器；

- 配置代码简洁且容易上手。

## Nginx 工作原理

Ngi nx 的模块从功能上分为如下三类。

- handlers （处理器模块）：此类模块直接处理请求，并进行输出内容和修改 headers

息等操 handlers 处理器模块 般只能有一个。

- filters （过滤器模块）：此类模块主要对其他处理器模块输出的内容进行修改操作，最

后由 Nginx 输出

- proxies （代理类模块）：此类模块是 Nginx HTTP upstream 之类的模块，这些模

块主要与后端一些服务比如 FastCGI 等进行交互，实现服务代理和负载均衡等功能

<img title="" src="file:///C:/Users/kayak-bj--0886/AppData/Roaming/marktext/images/2023-10-16-15-54-32-1697442867948.png" alt="" width="375" data-align="center">

Nginx 的高并发得益于其采用了 epoll 模型。epoll 模型的特点为 epoll 对于句柄事件的选择不是遍历的，是事件响应的，就句柄上事件来就马上选择出来，不需要遍历整个句柄链表，因此效率非常高。

<img title="" src="file:///C:/Users/kayak-bj--0886/AppData/Roaming/marktext/images/2023-10-16-15-57-07-1697443023654.png" alt="" width="366" data-align="center">
