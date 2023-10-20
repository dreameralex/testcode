# LNMP应用环境

LNMP或LEMP，其中LNMP为Linux、Nginx、MySQL、PHP等首字母的缩写，而LEMP中的E则表示Nginx

当LNMP组合工作时，首先是用户通过浏览器输入域名请求Nginx Web服务，如果请求是静态资源，则由Nginx解析返回给用户；如果是动态请求（.php结尾），那么Nginx就会把它通过FastCGI接口（生产常用方法）发送给PHP引擎服务（FastCGI进程php-fpm）进行解析，如果这个动态请求要读取数据库数据，那么PHP就会继续向后请求MySQL数据库，以读取需要的数据，并最终通过Nginx服务把获取的数据返回给用户，这就是LNMP环境的基本请求顺序流程。这个请求流程是企业使用LNMP环境的常用流程。



![](https://pic2.zhimg.com/v2-8d884a67257f422b01170495bacc4491_r.jpg)



第一步：用户在浏览器输入域名或者IP访问网站

第二步：用户在访问网站的时候，向web服务器发出http request请求，服务器响应并处理web请求，返回静态网页资源，如CSS、picture、video等，然后缓存在用户主机上。

第三步：服务器调用动态资源，PHP脚本调用fastCGI传输给php-fpm，然后php-fpm调用PHP解释器进程解析PHP脚本。

第四步：出现大流量高并发情况，PHP解析器也可以开启多进程处理高并发，将解析后的脚本返回给php-fpm，然后php-fpm再调给fast-cgi将脚本解析信息传送给nginx，服务器再通过http response传送给用户浏览器。

第五步：浏览器再将服务器传送的信息进行解析与渲染，呈现给用户。







# FastCGI介绍

CGI的全称为“通用网关接口”（Common Gateway Interface），为HTTP服务器与其他机器上的程序服务通信交流的一种工具，CGI程序须运行在网络服务器上。

FastCGI是一个可伸缩地、高速地在HTTP服务器和动态脚本语言间通信的接口（在Linux下，FastCGI接口即为socket，这个socket可以是文件socket，也可以是IP socket），主要优点是把动态语言和HTTP服务器分离开来。多数流行的HTTP服务器都支持FastCGI，包括Apache、Nginx和Lighttpd等。

- HTTP服务器和动态脚本语言间通信的接口或工具。

- 可把动态语言解析和HTTP服务器分离开。

- Nginx、Apache、Lighttpd，以及多数动态语言都支持FastCGI。

- FastCGI接口方式采用C/S结构，分为客户端（HTTP服务器）和服务器端（动态语言解析服务器）。

- PHP动态语言服务器端可以启动多个FastCGI的守护进程（例如php-fpm（fcgi process mangement））。

- HTTP服务器通过（例如Nginx fastcgi_pass）FastCGI客户端和动态语言FastCGI服务器端通信（例如php-fpm）。

# Appendix

## Socket

**套接字（socket）是一个抽象层，应用程序可以通过它发送或接收数据，可对其进行像对文件一样的打开、读写和关闭等操作。套接字允许应用程序将I/O插入到网络中，并与网络中的其他应用程序进行通信。网络套接字是IP地址与端口的组合。**

可以把Socket编程理解为对TCP协议的具体实现。现在解释清楚什么是Socket后，我相信现在无论是Java还是C#，或是其它语言，你都可以对于Socket编程轻松地上手了。

# Radis
