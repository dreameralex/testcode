# 监视磁盘使用情况
找出某个文件（或多个文件）占用的磁盘空间：
$ du FILENAME1 FILENAME2 ..
例如：
$ du file.txt
要获得某个目录中所有文件的磁盘使用情况，并在每一行中显示各个文件的具体详情，可以使用：
$ du -a DIRECTORY

## 选择查看文件大小
$ du -k FILE(s)
$ du -m FILE(s)

## 从磁盘使用统计中排除部分文件


* 指定文件删除
选项--exclude可以与通配符或单个文件名配合使用
du --exclude "WILDCARD" DIRECTORY
选项--exclude会排除匹配模式的一个或多个文件。选项--exclude-from能够排除多个文件或模式。每个文件名或模式必须独占一行。

* 按类别删除
	ls *.txt >EXCLUDE.txt
	du --exclude-from EXCLUDE.txt DIRECTORY

- 递归深度
	du --max-depth 2 DIRECTORY


## 找出指定目录中最大的10个文件
	du -ak SOURCE_DIR | sort -nrk 1 | head
选项-a可以显示出SOURCE_DIR中所有文件和目录的大小。输出的第一列就是文件大小。选项-k
表示以KB为单位。第二列包含文件或目录的名称。
	sort -nrk 1 
sort的选项-n指明按数值排序，选项-l和-r指明对第一列按逆序排序。head用来从输出中提取前10行
\-n是按照数字大小排序，-r是以相反顺序，-k是指定需要爱排序的栏位

[[Shell——技巧#排序]]


这个单行脚本的缺点之一在于它的结果中还包含了目录。我们可以使用find命令改进脚本，使
其只输出最大的文件：
	$ find . -type f -exec du -k {} \; | sort -nrk 1 | head
[[Shell——技巧#Find]]

## 磁盘可用信息

du提供磁盘使用情况信息，而df提供磁盘可用空间信息。df的-h选项会以易读的格式输出磁盘空间信息。

# 计算命令执行时间

- time命令可以测量出应用程序的执行时间：
	$ time APPLICATION
- 选项-o可以将相关的时间统计信息写入文件：
	$ /usr/bin/time -o output.txt COMMAND

- 选项-f可以指定输出哪些统计信息及其格式。格式字符串包括一个或多个以%为前缀的
参数。格式参数包括以下几种。
 real时间: %e
	Real：指的是壁钟时间（wall clock time），也就是命令从开始执行到结束的时间。这段时
间包括其他进程所占用的时间片（time slice）以及进程被阻塞时所消耗的时间（例如，为
等待I/O操作完成所用的时间）。

 user时间: %U
	User：是指进程花费在用户模式（内核模式之外）中的CPU时间。这是执行进程所花费
的时间。执行其他进程以及花费在阻塞状态中的时间并没有计算在内。
 sys时间: %S
	Sys：是指进程花费在内核中的CPU时间。它代表在内核中执行系统调用所使用的时间，
这和库代码（library code）不同，后者仍旧运行在用户空间。与“user时间”类似，这也
是真正由进程使用的CPU时间。参考表9-1，其中简要描述了内核模式（也称为监督模式）
和系统调用机制。
 系统分页大小：%Z

|  参数  |  描述  |  
|  ---  |  ---  |  
|  %C  |  被计时的命令名称以及命令行参数  | 
|  %D  |  进程非共享数据区的平均大小，以KB为单位  | 
|  %E  |  进程使用的real时间（壁钟时间），显示格式为[小时:]分钟:秒  | 
|  %x  |  命令的退出状态  | 
|  %k  |  进程接收到的信号数量  | 
|  %W  |  进程被交换出主存的次数  | 
|  %Z  |  以字节为单位系统的页面大小。这是一个系统常量，但在不同的系统中，这个常量值也不同  | 
|  %P  |  进程所获得的CPU时间百分比。这个值等于user + system时间除以总运行时间。结果以百分比形式显示  | 
|  %K  |  进程的平均总内存使用量（data+stack+text），以KB为单位  | 
|  %w  |  进程主动进行上下文切换的次数，例如等待I/O操作完成  | 
|  %c  |  进程被迫进行上下文切换的次数（由于时间片到期）  | 
 
# 收集登录用户、启动日志及启动故障的相关信息
- who命令可以获取当前登录用户的相关信息:
-  w命令可以获得有关登录用户更详细的信息:
- users命令只列出当前的登录用户列表:
- uptime命令可以查看系统的加电运行时长:
- last命令可以获取自文件/var/log/wtmp创建之后登录过系统的用户列表。这可能会追溯
到一年之前(甚至更久):
last命令也可以获取指定用户信息
上述命令中的USER可以是系统真实用户
- lastb命令可以获取失败的用户登录会话信息

# 列出 1 小时内占用 CPU 最多的 10 个进程

ps命令能够显示出系统中进程的详细信息。这些信息包括CPU使用情况、所执行的命令、内
存占用、进程状态等。

	 #!/bin/bash
	 #文件名: pcpu_usage.sh
	#用途:计算1个小时内进程的CPU占用情况
	#将SECS更改成需要进行监视的总秒数
	#UNIT_TIME是取样的时间间隔,单位是秒
	SECS=60
	UNIT_TIME=60
	STEPS=$(( $SECS / $UNIT_TIME ))
	echo Watching CPU usage... ;
	#采集数据,存入临时文件
	for((i=0;i<STEPS;i++))
	do
		ps -eocomm,pcpu | egrep -v '(0.0)|(%CPU)' >> /tmp/cpu_usage.$$
		sleep $UNIT_TIME
	done
	echo
	echo CPU eaters :
	cat /tmp/cpu_usage.$$ | \
	awk '
	{ process[$1]+=$2; }
	END{
		for(i in process)
		{
			printf("%-20s %s\n",i, process[i]) ;
		}
		}' | sort -nrk 2 | head
	#删除临时日志文件
	rm /tmp/cpu_usage.$$


-  ps -eocomm,pcpu 就会产生一份系统活动报告
- egrep -v '(0.0)
`-v, --invert-match`: 反转匹配的意义，以选择不匹配的行。

[[Shell——技巧#grep]]




# 使用 watch 监视命令输出
watch 命令会按照指定的间隔时间来执行命令并显示其输出。你可以使用终端会话和第10章
中描述的screen命令创建一个自定义的控制面板(dashboard),利用watch监视系统的运行情况

# 记录文件及目录访问情况
	#/bin/bash
	#文件名: watchdir.sh
	#用途:监视目录访问
	path=$1
	#将目录或文件路径作为脚本参数
	inotifywait -m -r -e create,move,delete $path  -q

inotifywait能够监视的事件见

|  事件|描述|
| ---|---|
|access|读取文件|
|modify|文件内容被修改|
|attrib|文件元数据被修改|
|move|文件移动操作|
|create|创建新文件|
|open|文件打开操作|
|close|文件关闭操作|
|delete|文件被删除|


# 使用 syslog 记录日志
与守护进程和系统进程相关的日志文件位于/var/log目录中。在Linux系统中,由守护进程
sylogd使用syslog标准协议处理日志。每一个标准应用程序都可以利用syslogd记录日志。

系统日志服务是由一个名为syslog的服务管理的，如一下日志文件都是由syslog日志服务驱动的：  
　　/var/log/lastlog ：记录最后一次用户成功登陆的时间、登陆IP等信息  
　　/var/log/messages ：记录Linux操作系统常见的系统和服务错误信息  
　　/var/log/secure ：Linux系统安全日志，记录用户和工作组变坏情况、用户登陆认证情况  
　　/var/log/btmp ：记录Linux登陆失败的用户、时间以及远程IP地址  
　　/var/log/cron ：记录crond计划任务服务执行情况

- 向日志文件/var/log/messages中写入信息:
	logger This is a test log line
	tail -n 1 /var/log/messages



- 选项-t可以定义消息标签:

#  使用 logrotate 管理日志文件
logrotate能够限制日志文件的大小。系统的日志记录程序将信息添加到日志文件的同时并不会删除先前的数据。日志文件因此会变得越来越大。logrotate 命令根据配置文件扫描特定的日志文件。它只保留文件中最近添加的100KB内容(假设指定了SIZE = 100k),将多出的数据(旧的日志数据)不断移入新文件logfile_name.1。当该文件(logfile_name.1)中的内容超出了SIZE的 限 定 , logrotate 会 将 其 重 命 名 为 logfile_name.2 并 再 创 建 一 个 新 的 logfile_name.1 1。logrotate命令还会将旧的日志文件压缩成logfile_name.1.gz、logfile_name.2.gz,以此类推。


|  事件|描述|
| ---|---|
|  missingok|如果日志文件丢失,则忽略并返回(不对日志文件进行轮替)|
|  notifempty|仅当源日志文件非空时才对其进行轮替|
|  size 30k|限制执行轮替的日志文件的大小。可以用1M表示1MB|
|  compress|允许用gzip压缩旧日志文件|
|  weekly|指定执行轮替的时间间隔。可以是weekly、monthly、yearly或daily|
|  rotate 5|   需要保留的旧日志文件的归档数量。在这里指定的是5,所以这些文件名将会是program.log.1.gz、program.log.2.gz ... program.log.5.gz |
|  create 0600 root root| 指定所要创建的归档文件的权限、用户以及用户组|

# 通过监视用户登录找出入侵者


# 确定系统中用户的活跃时段



