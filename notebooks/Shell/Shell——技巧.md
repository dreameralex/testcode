# 循环并行执行
可将循环体放入()&。()中的命令会在子shell中运行，而&会将其置入后台

# Sort

-o<输出文件>：将排序后的结果存入制定的文件；  
-r：以相反的顺序来排序；  
-t<分隔字符>：指定排序时所用的栏位分隔字符  
-n：依照数值的大小排序；  
-b：忽略每行前面开始出的空格字符；  
-c：检查文件是否已经按照顺序排序；  
-d：排序时，处理英文字母、数字及空格字符外，忽略其他的字符；  
-f：排序时，将小写字母视为大写字母；  
-i：排序时，除了040至176之间的ASCII字符外，忽略其他的字符；  
-m：将几个排序号的文件进行合并；  
-M：将前面3个字母依照月份的缩写进行排序；  
+<起始栏位>-<结束栏位>：以指定的栏位来排序，范围由起始栏位到结束栏位的前一栏位。

# exces

`exec` 是一个用于替换进程或执行命令的 Shell 内建命令。它可以将当前脚本的执行环境替换为新的进程，并在执行新进程后不返回到原始脚本。简单来说，`exec` 命令用于取代当前进程并执行新的命令。

使用 `exec` 命令可以实现以下功能:
- 执行一个新的命令并替换当前进程。
- 将当前脚本的执行环境传递给新的进程。
- 在脚本中启动后台进程并将控制权转移到后台进程。


在使用 `exec` 命令时，需要注意以下几点：

1. `exec` 命令将取代当前进程并执行新的命令，因此在 `exec` 命令之后的脚本代码将不再执行。
2. 如果 `exec` 命令执行失败，脚本将继续执行后续的命令。
3. 使用 `exec` 命令时，要确保正确处理输入和输出，以避免意外的结果或数据丢失。
4. 当使用 `exec` 命令替换当前进程时，新进程将继承原始脚本的文件描述符和环境变量。这可以让新进程访问和操作与原始脚本相关的资源。
5. 如果在脚本中使用 `exec` 命令启动后台进程，要确保在适当的时候结束或清理后台进程，以避免资源泄露或不必要的进程运行。

# Find

二、find命令的基本格式：find  [路径]  [选项] [操作]

三、find 命令常用的选项：
name                              :根据文件名查找文件
perm                             ：根据文件的权限查找文件
prune                             ：可以使find命令不在当前指定的目录中查找
user                               ：根据文件属主查找文件
group                             ：根据文件所属的用户组查找文件
mtime -n +n :                  :根据文件更改的时间查找，-n 表示文件更改时间距今在n天之内，+n 表示文件更改时间距今在n天前
-newer file1 ! file2           :查找更改时间比文件file1新 但比文件file2 旧的文件
type  （b：块设备文件，d:目录，c：字符设备文件，p：管道文件，l：符号链接文件，f：普通文件

四、find命令操作名称
print ：将匹配的文件输出到标准输出
exec：对匹配的文件执行该参数所给出的shell命令。相应命令形式为‘command’ {} \;。  //注意{}与\;之间有空格  

ok：与exec基本相同


# grep
## 1. grep简介

grep （global search regular expression_r(RE) and print out the line,全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。Unix的grep家族包括grep、egrep和fgrep。egrep和fgrep的命令只跟grep有很小不同。  
**egrep是grep的扩展，支持更多的re元字符**， fgrep就是fixed grep或fast grep，它们把所有的字母都看作单词，也就是说，正则表达式中的元字符表示回其自身的字面意义，不再特殊。linux使用GNU版本的grep。它功能更强，可以通过-G、-E、-F命令行选项来使用egrep和fgrep的功能。

grep的工作方式是这样的，它在一个或多个文件中搜索字符串模板。如果模板包括空格，则必须被引用，模板后的所有字符串被看作文件名。搜索的结果被送到屏幕，不影响原文件内容。

grep可用于shell脚本，因为grep通过返回一个状态值来说明搜索的状态，如果模板搜索成功，则返回0，如果搜索不成功，则返回1，如果搜索的文件不存在，则返回2。我们利用这些返回值就可进行一些自动化的文本处理工作。

## 2. grep正则表达式元字符集（基本集）

`^`  
锚定行的开始 如：’^grep’匹配所有以grep开头的行。

`$`  
锚定行的结束 如：'grep$'匹配所有以grep结尾的行。

`.`  
匹配一个非换行符的字符如：'gr.p’匹配gr后接一个任意字符，然后是p。

`*`  
匹配零个或多个先前字符如：’*grep’匹配所有一个或多个空格后紧跟grep的行。 .*一起用代表任意字符。

`[]`  
匹配一个指定范围内的字符，如’[Gg]rep’匹配Grep和grep。

`[^]`  
匹配一个不在指定范围内的字符，如：’[^A-FH-Z]rep’匹配不包含A-R和T-Z的一个字母开头，紧跟rep的行。

…  
标记匹配字符，如’love’，love被标记为1。

`\<`  
锚定单词的开始，如:’<grep’匹配包含以grep开头的单词的行。

`\>`  
锚定单词的结束，如’grep>'匹配包含以grep结尾的单词的行。

`x\{m\}`  
重复字符x，m次，如：'0{5}'匹配包含5个o的行。

`x\{m,\}`  
重复字符x,至少m次，如：'o{5,}'匹配至少有5个o的行。

`x\{m,n\}`  
重复字符x，至少m次，不多于n次，如：'o{5,10}'匹配5–10个o的行。

`\w`  
匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p’匹配以G后跟零个或多个文字或数字字符，然后是p。

`\W`  
\w的反置形式，匹配一个或多个非单词字符，如点号句号等。

`\b`  
单词锁定符，如: '\bgrep\b’只匹配grep。

## 3. 用于egrep和 grep -E的元字符扩展集

`+`  
匹配一个或多个先前的字符。如：’[a-z]+able’，匹配一个或多个小写字母后跟able的串，如loveable,enable,disable等。

`?`  
匹配零个或多个先前的字符。如：'gr?p’匹配gr后跟一个或没有字符，然后是p的行。

`a|b|c`  
匹配a或b或c。如：grep|sed匹配grep或sed

`()`  
分组符号，如：love(able|rs)ov+匹配loveable或lovers，匹配一个或多个ov。

- `-A NUM, --after-context=NUM`: 在匹配行之后打印尾部上下文的`NUM`行，在相邻的匹配组之间放置包含`--`的行。
- `-a, --text`: 像处理文本一样处理二进制文件，这相当于`--binary files=text`选项。
- `-B NUM, --before-context=NUM`: 在匹配行之前打印前导上下文的`NUM`行，在相邻的匹配组之间放置包含`--`的行。
- `-C NUM, --context=NUM`: 打印输出上下文的`NUM`行，在相邻的匹配组之间放置包含`--`的行。
- `-b, --byte-offset`: 打印输入文件中每行输出之前的字节偏移量。
- `--binary-files=TYPE`: 如果文件的前几个字节指示该文件包含二进制数据，则假定该文件为类型类型。默认情况下，`TYPE`是`binary`，`grep`通常输出一行消息，说明二进制文件匹配，或者不输出消息(如果不匹配)。如果`TYPE`不匹配，`grep`假设二进制文件不匹配，这相当于`-I`选项。如果`TYPE`是`text`，`grep`会像处理文本一样处理二进制文件，这相当于`-a`选项。此外`grep--binary files=text`可能会输出二进制垃圾，如果输出是终端，并且终端驱动程序将其中的一些解释为命令，则会产生严重的副作用。
- `--colour[=WHEN], --color[=WHEN]`: 在匹配字符串周围加上标记`find in GREP_COLOR`环境变量，`WHEN`可以是`never`、`always`、`auto`。
- `-c, --count`: 禁止正常输出，而是为每个输入文件打印匹配行的计数，使用`-v，--invert match`选项，计算不匹配的行数。
- `-D ACTION, --devices=ACTION`: 如果输入文件是设备、`FIFO`或套接字，使用`ACTION`来处理它。默认情况下，`ACTION`是`read`，这意味着设备的读取就像它们是普通文件一样，如果`ACTION`为`skip`，则设备将自动跳过。
- `-d ACTION, --directories=ACTION`: 如果输入文件是目录，使用`ACTION`来处理它。默认情况下，`ACTION`是`read`，这意味着目录的读取就像它们是普通文件一样，如果`ACTION`是`skip`，则目录将被自动跳过，如果`ACTION`是递归的，`grep`将递归地读取每个目录下的所有文件，这相当于`-r`选项。
- `-e PATTERN, --regexp=PATTERN`: 使用`PATTERN`作为模式，用于保护以`-`开头的模式。
- `-F, --fixed-strings`: 将`PATTERN`解释为固定字符串的列表，用换行符分隔，这些字符串可以匹配。
- `-P, --perl-regexp`: 将`PATTERN`解释为`Perl`正则表达式。
- `-f FILE, --file=FILE`: 从`FILE`获取模式，每行一个，空文件包含零个模式，因此不匹配。
- `-G, --basic-regexp`: 将`PATTERN`解释为基本正则表达式，这是默认值。
- `-H, --with-filename`: 打印每个匹配项的文件名。
- `-h, --no-filename`: 当搜索多个文件时，禁止在输出中使用文件名前缀。
- `--help`: 显示帮助文件。
- `-I`: 处理二进制文件，就像它不包含匹配数据一样，这相当于`--binary-files=without-match`选项。
- `-i, --ignore-case`: 忽略`PATTERN`和输入文件中的大小写区别。
- `-L, --files-without-match`: 禁止正常输出，而是打印通常不会从中打印输出的每个输入文件的名称，扫描将在第一个匹配时停止。
- `-l, --files-with-matches`: 禁止正常输出，而是打印通常从中打印输出的每个输入文件的名称，扫描将在第一个匹配时停止。
- `-m NUM, --max-count=NUM`: 在匹配行数之后停止读取文件。如果输入是来自常规文件的标准输入，并且输出`NUM`个匹配行，`grep`确保标准输入在退出之前定位到最后一个匹配行之后，而不管是否存在后续上下文行。这使调用进程能够继续(恢复)搜索，当`grep`在NUM个匹配行之后停止时，它输出任何后面的上下文行。当`-c`或`--count`选项也被使用时，`grep`不会输出大于`NUM`的计数。当`-v`或`--invert match`选项也被使用时，`grep`会在输出`NUM`个不匹配的行之后停止。
- `--mmap`: 如果可能，使用`mmap`系统调用来读取输入，而不是默认的读取系统调用。在某些情况下，`--mmap`可以产生更好的性能。但是，如果在`grep`操作时输入文件收缩，或者发生`I/O`错误，那么`--mmap`可能会导致未定义的行为(包括核心转储)。
- `-n, --line-number`: 在输出的每一行前面加上输入文件中的行号。
- `-o, --only-matching`: 只显示匹配行中与模式匹配的部分。
- `--label=LABEL`: 将实际来自标准输入的输入显示为来自文件`LABEL`的输入。这对于`zgrep`之类的工具尤其有用，例如`gzip -cd foo.gz | grep -H --label = foo`。
- `--line-buffered`: 使用行缓冲，这可能会导致性能损失。
- `-q, --quiet, --silent`: 保持安静，不向标准输出写入任何内容。如果找到任何匹配项，即使检测到错误，也立即退出，状态为零。
- `-R, -r, --recursive`: 递归地读取每个目录下的所有文件，这相当于`-d recurse`选项。
- `-s, --no-messages`: 禁止显示有关不存在或不可读文件的错误消息。
- `-U, --binary`: 将文件视为二进制文件。默认情况下，在`MS-DOS`和`MS Windows`下，`grep`通过查看从文件中读取的第一个`32KB`的内容来猜测文件类型。如果`grep`确定文件是文本文件，它将从原始文件内容中删除`CR`字符(以使带有`^`和`$`的正则表达式正常工作)。指定`-U`会推翻这种猜测，导致读取所有文件并逐字传递给匹配机制，如果文件是一个文本文件，每行末尾都有`CR/LF`对，这将导致某些正则表达式失败。此选项对`MS-DOS`和`MS Windows`以外的平台无效。
- `-u, --unix-byte-offsets`: 报告`Unix`样式的字节偏移量，此开关使`grep`报告字节偏移，就好像该文件是`Unix`样式的文本文件一样，即去除了`CR`字符。这将产生与在`Unix`机器上运行`grep`相同的结果，除非也使用`-b`选项，否则该选项无效。它对除`MS-DOS`和`MS-Windows`以外的平台没有影响。
- `-V, --version`: 输出版本信息。
- `-v, --invert-match`: 反转匹配的意义，以选择不匹配的行。
- `-w, --word-regexp`: 只选择与表单中包含的单词匹配的行。测试是匹配的子串必须在行的开头，或者前面有非单词组成字符，同样，它必须位于行的末尾，或者后跟非单词组成字符。单词组成字符是字母、数字和下划线。
- `-x, --line-regexp`: 仅选择与整行完全匹配的那些匹配项。
- `-Z, --null`: 输出零字节(`ASCII NULL`字符)，而不是通常在文件名后的字符。例如`grep -lZ`在每个文件名之后输出一个零字节，而不是通常的换行符。即使存在包含不寻常字符(例如换行符)的文件名，此选项也可以使输出明确。此选项可与`find -print0`、`perl -0`、`sort -z`和`xargs -0`等命令一起使用，以处理任意文件名，即使是包含换行符的文件名。

# if
## -n
-n  字符串的长度>0    (注意变量要加双引号"") 

	if [ -n "$1" ];
	then
	  year=$1
	else  
	  year=`date +%Y`
	fi
