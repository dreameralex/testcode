# 仅执行一次的计划任务

单一计划任务`atd`

`at`命令产生要运行的任务，并将这个任务以文本文件的方式写入/var/spool/at/目录内，该任务便可以等待`atd`这个服务的使用与执行了。

at工作流程：

- 查找/etc/at.allow是否有，有则执行

- 查找/etc/at.deny是否有，有则不执行，否则可以执行

- 都不存在只有root可以使用`at`

## 操作：

- -f：指定包含具体指令的任务文件 

- -q：指定新任务的队列名称 

- -l：显示待执行任务的列表 

- -d：删除指定的待执行任务 

- -m：任务执行完成后向用户发送 E-mail

### 删除at命令

可以使用`atq`与`atrm`删除

`atq`：查询命令

`atrm`：删除

### 执行空闲运行

`batch`：系统有空时后台运行（CPU负载小于0.8时，运行）

## 示例

你想在早上 11:20 的时候，在 `at-test.txt` 文档里写入 `hello world`

```shell
at 11:20 AM
warning: commands will be executed using /bin/sh
at> echo "hello world" > ~/at-test.txt
at> <EOT>
job 3 at Mon Jul 26 11:20:00 2021
```

# 循环执行的计划任务

`crontab`建立循环执行任务，任务记载至/var/spool/cron/中。

```shell
crontab [-u username] [-l|-e|-r]
crontab [ -u user ] file
```

- -e : 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe)
- -r : 删除目前的时程表
- -l : 列出目前的时程表

时间格式如下：

```shell
f1 f2 f3 f4 f5 program
```

![](C:\Users\kayak-bj--0886\AppData\Roaming\marktext\images\2023-10-13-10-54-52-1697165689803.png)

## 配置系统文件

cron服务每分钟检测，读取/etc/crontab与/var/spool/cron的内容，所以编辑完/etc/crontab这个文件，并保存后，cron可以自动执行。

# 可唤醒停机期间的工作任务

anacron 会以 1 天、1周（7天）、一个月作为检测周期，判断是否有定时任务在关机之后没有执行。如果有这样的任务，那么 anacron 会在特定的时间重新执行这些定时任务。

那么，anacron 是如何判断这些定时任务已经超过执行时间的呢？这就需要借助 anacron 读取的时间记录文件。anacron 会分析现在的时间与时间记录文件所记载的上次执行 anacron 的时间，将两者进行比较，如果两个时间的差值超过 anacron 的指定时间差值（一般是 1 天、7 天和一个月），就说明有定时任务没有执行，这时 anacron 会介入并执行这个漏掉的定时任务，从而保证在关机时没有执行的定时任务不会被漏掉。

## 语法

```shell
anacron [-sfn] [job]..
anacron -u [job]
```

| f   | 强制执行相关工作，忽略时间戳。                                                          |
| --- | ------------------------------------------------------------------------ |
| -u  | 更新 /var/spool/anacron/cron.{daily，weekly，monthly} 文件中的时间戳为当前日期，但不执行任何工作。 |
| -s  | 依据 /etc/anacrontab 文件中设定的延迟时间顺序执行工作，在前一个工作未完成前，不会开始下一个工作。                |
| -n  | 立即执行 /etc/anacrontab 中所有的工作，忽略所有的延迟时间。                                   |
| -q  | 禁止将信息输出到标准错误，常和 -d 选项合用。                                                 |
