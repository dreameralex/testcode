# 监视磁盘使用情况

`du`（disk usage）和`df`（disk free）命令可以报告磁盘使用情况。这两个工具能够统计出文件和目录的磁盘占用情况以及可用的磁盘空间.

- -a: 递归地输出指定目录或多个目录中所有文件的统计结果

- -c可以计算出文件或目录所占用的总的磁盘空间，另外还会输出单个文件的大小

- -b、-k和-m可以强制du使用特定的单位打印磁盘使用情况。

## 显示磁盘使用总计

```shell
du FILENAME1 FILENAME2 ..

du -a test
```

### 排除文件

选项--exclude和--exclude-from可以让du在磁盘使用统计中排除部分文件。

### 找出指定目录中最大的10个文件

```shell
du -ak SOURCE_DIR | sort -nrk 1 | head

find . -type f -exec du -k {} \; | sort -nrk 1 | head
利用find替du将文件过滤出来，这样就无需使用du遍历文件系统了。
```

选项-a可以显示出SOURCE_DIR中所有文件和目录的大小。输出的第一列就是文件大小。选项-k 表示以KB为单位。第二列包含文件或目录的名称。

sort的选项-n指明按数值排序，选项-l和-r指明对第一列按逆序排序。head用来从输出中

提取前10行：

## 磁盘可用空间信息

df提供磁盘可用空间信息。df的-h选项会以易读的格式输出磁盘空间信息。

# 运行管理

## 计算命令执行时间

```shell
 time APPLICATION
```

选项-o可以将相关的时间统计信息写入文件

```shell
 /usr/bin/time -o output.txt COMMAND
```

选项-a可以配合-o使用，将命令执行时间追加到原文件的末尾

```shell
 /usr/bin/time -a -o output.txt COMMAND
```

选项-f可以指定输出哪些统计信息及其格式。格式字符串包括一个或多个以%为前缀的

参数。

- real时间: %e

- user时间: %U

- sys时间: %S

- 系统分页大小：%Z

```shell
 /usr/bin/time -f "FORMAT STRING" COMMAND
 /usr/bin/time -f "Time: %U" -a -o timing.log uname
```

## 收集登录用户、启动日志及启动故障的相关信息

- who命令可以获取当前登录用户的相关信息

- w命令可以获得有关登录用户更详细的信息

- users命令只列出当前的登录用户列表

- uptime命令可以查看系统的加电运行时长

- last命令可以获取自文件/var/log/wtmp创建之后登录过系统的用户列表。这可能会追溯

- 到一年之前（甚至更久）

- last命令也可以获取指定用户信息

- lastb命令可以获取失败的用户登录会话信息

## 使用Watch监视命令输出

```shell
watch COMMAND
```

## 记录文件及目录访问情况

inotifywait命令会监视文件或目录并报告何时发生了某种事件。

```shell
inotifywait -m -r -e create,move,delete $path -q
```

# syslog 记录日志

与守护进程和系统进程相关的日志文件位于/var/log目录中。在Linux系统中，由守护进程

sylogd使用syslog标准协议处理日志。

| 项目                  | 解释              |
| ------------------- | --------------- |
| /var/log/boot.log   | 系统启动信息          |
| /var/log/httpd      | Apache Web服务器日志 |
| /var/log/messages   | 内核启动信息          |
| /var/log/auth.log   | 用户认证日志          |
| /var/log/secure     |                 |
| /var/log/dmesg      | 系统启动信息          |
| /var/log/mail.log   | 邮件服务器日志         |
| /var/log/maillog    |                 |
| /var/log/Xorg.0.log | X服务器日志          |

在脚本中可以使用logger命令创建及管理日志

## 向日志文件/var/log/messages中写入信息

```shell
logger LOG_MESSAGE
```

/var/log/messages是一个通用日志文件。如果使用logger命令，它默认将日志写入

/var/log/messages中。

## 选项-t可以定义消息标签

选项-p和/etc/rsyslog.d/目录下的配置文件决定了日志消息保存到何处。

如果需要保存到指定的文件中，请按照以下步骤操作：

- 在/etc/rsyslog.d/下创建一个新的配置文件；

- 在配置文件中添加模式并指定日志文件；

- 重启日志守护进程（syslogd）。

## 选项-f可以将其他文件的内容记录到系统日志中

# 进程管理

## 进程与程序

[Alt] + [F1]......可以切换终端

```shell
cp file1 file2 &
```

& 将file1复制到file2，并放在后台执行；



## 任务管理

查看后台任务状态`jobs`

```shell
jobs [-lrs6]
```

- -l：列出job number与命令串之外，同时列出PID的号码；

- -r：仅列出正在后台run的任务；

- -s：仅列出后台当中暂停的任务；

后台任务拿到前台处理`fg`

```shell
fg %jobnumber
```

## 进程的管理

### 查看进程

#### `ps`查看进程

- -A 列出所有的进程  

- -w 显示加宽可以显示较多的资讯  

- -au 显示较详细的资讯  

- -aux 显示所有包含其他使用者的行程



```shell
1 登录PID的相关信息列示
ps -l
2 列出正在内存中的进程
ps aux
3 显示所有进程
ps -lA
4 列出类似进程书的进程展示
ps axjf
5 找出cron与rsyslog两个服务有关的PID号码
ps aux|egrep `(cron|rsyslog)`
```

#### `top`动态查看进程变化

```shell
top [-d 数字] | top [-bnp]
```

| -b  | 以批次的方式执行top                |
| --- | -------------------------- |
| -n  | 与-b配合使用，表示需要进行几次top命令的输出结果 |
| -p  | 指定特定的pid进程号进行观察            |



`top`执行中的按键命令

| 参数  | 含义                      |
| --- | ----------------------- |
| ？   | 显示在top当中可以输入的命令         |
| P   | 以CPU的使用资源排序显示           |
| M   | 以内存的使用资源排序显示            |
| N   | 以pid排序显示                |
| T   | 由进程使用的时间累计排序显示          |
| k   | 给某一个pid一个信号,可以用来杀死进程(9) |
| r   | 给某个pid重新定制一个nice值（即优先级) |
| q   | 退出top（用ctrl+c也可以退出top）  |

#### `pstree`

```shell
pstree [A|U] [-up]
```

-A: 各进程树之间的连接以ASCII码字符来连接  
-U:各进程树之间的连接以utf8字符来连接，某些终端可能会有错误  
-p:同时列出每个进程的PID  
-u: 同时列出每个进程的所属账号名称：

### 进程的管理

#### `kill -gignal PID`

### `killall [-iIe] [command name]`

### 关于进程执行的顺序

#### `nice`

执行新的命令给与新的nice值

#### `renice`

已经存在的进程的nice重新调整

### 查看系统资源信息

#### `free`查看内存使用情况

#### `unname`查看系统与内核相关信息

#### `uptime`：查看系统启动时间与任务载荷

#### `netstat`：追踪网络或者socket文件



#### `dmesg`：分析内核产生的信息

#### `vmstst`：检测系统资源变化

## 特殊文件与进程

### 具有SUID/SGID权限的命令执行状态

#### suid权限

作用：让普通用户临时拥有该文件的属主的执行权限，suid权限只能应用在二进制可执行文件（命令）上，而且suid权限只能设置在属主位置上。

suid权限的去除可以使用数字或字母的形式添加，如果使用数字，0表示去除权限，4表示添加权限，而且是在原权限的数字表达形式开头加0或4



#### sgid权限

作用：sgid权限一般应用在目录上，当一个目录拥有sgid权限时，任何用户在该目录下创建的文件的属组都会继承该目录的属组。

查询整个系统的SUID/SGID的文件

```shell
find / -perm /6000
```

### /proc/*代表的意义

内存数据写入/proc/*这个目录下

查看/proc

```shell
11 /porc
```

### 查询已使用文件或已执行进程使用的文件

#### `fuser`借由文件找出正在使用该文件的进程

#### `lsof`：列出被进程所使用的文件名称

#### `pidof`：找出某几个正在执行的进程的PID



## SElinux





















## 收集进程信息
