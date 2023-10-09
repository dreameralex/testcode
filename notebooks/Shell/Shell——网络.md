# 网络设置
## 名字与服务器DNS 
cat /etc/resolv.conf
名字服务器是在文件 /etc/resolv.conf中定义的：

我们可以编辑该文件来手动添加名字服务器或是使用下面的命令：
/# sudo echo nameserver IP_ADDRESS >> /etc/resolv.conf

### 获取域名IP
ping baidu.com

## DNS查找
host命令会列出某个域名所有的IP地址
host baidu.com
nslookup命令可以完成名字与IP地址之间的相互映射：

## 显示路由表信息
route

route -n

# ping
ping命令使用Internet控制消息协议（Internet Control Message Protocol，ICMP）中的echo分
组检验网络上两台主机之间的连通性。当向某台主机发送echo分组时，如果分组能够送达且该
主机处于活动状态，那么它就会返回一条回应（reply）。如果没有通往目标主机的路由或是目标
主机不知道如何将回应返回给请求方，ping命令则执行失败。

ping 192.168.0.1

### 限制发送分组数量
ping xxx -c n

### ping命令的返回状态
ping domain -c2
if [ $? -eq0 ];
then
echo Successful ;
else
echo Failure
fi

# 跟踪 IP 路由

traceroute destinationIP
destinationIP可以是IP地址，也可以是域名。

# 使用 SSH 在远程主机上执行命令
