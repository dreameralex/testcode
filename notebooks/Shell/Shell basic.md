# 正则表达式

## 规则

### 位置标记

^：指定了匹配正则表达式的文本必须起始于字符串的首部，^tux 能够匹配以tux起始的行

$：指定了匹配正则表达式的文本必须结束于目标字符串的尾部，tux \$能够匹配以tux结尾的行

### 标识符

| 正则表达式 | 描 述                               | 示 例                                                  |
| ----- | --------------------------------- | ---------------------------------------------------- |
| A字符   | 正则表达式必须匹配该字符                      | A能够匹配字符A                                             |
| .     | 匹配任意一个字符                          | Hack.能够匹配Hackl和Hacki，但是不能匹配Hackl2或Hackil，它只能匹配单个字符   |
| []    | 匹配中括号内的任意一个字符。中括号内可以是一个字符组或字符范围   | coo[kl]能够匹配cook或cool，[0-9]匹配任意单个数字                   |
| [^]   | 匹配不在中括号内的任意一个字符。中括号内可以是一个字符组或字符范围 | 9[^01]能够匹配92和93，但是不匹配91和90；A[^0-9]匹配A以及随后除数字外的任意单个字符 |

### 数量修饰符

| 正则表达式 | 描 述                 | 示 例                                              |
| ----- | ------------------- | ------------------------------------------------ |
| ?     | 匹配之前的项1次或0次         | colou?r能够匹配color或colour，但是不能匹配colouur            |
| +     | 匹配之前的项1次或多次         | Rollno-9+能够匹配Rollno-99和Rollno-9，但是不能匹配Rollno-    |
| *     | 匹配之前的项0次或多次         | co*l能够匹配cl、col和coool                             |
| {n}   | 匹配之前的项n次            | [0-9]{3}能够匹配任意的三位数，[0-9]{3}可以扩展为[0-9][0-9] [0-9] |
| {n,}  | 之前的项至少需要匹配n次        | [0-9]{2,} 能够匹配任意一个两位或更多位的数字                      |
| {n,m} | 之前的项所必须匹配的最小次数和最大次数 | [0-9]{2,5}能够匹配两位数到五位数之间的任意一个数字                   |

### 其他

| 正则表达式 | 描 述               | 示 例                               |
| ----- | ----------------- | --------------------------------- |
| ()    | 将括号中的内容视为一个整体     | ma(tri)?x 能够匹配max或matrix          |
|       |                   | 指定了一种选择结构，可以匹配 \| 两边的任意一项         |
| \     | 转义字符可以转义之前介绍的特殊字符 | a\.b能够匹配a.b，但不能匹配ajb。因为\忽略了.的特殊意义 |

# grep搜索

grep能够接受正则表达式，生成各种格式的输出。

- a --text # 不要忽略二进制数据。 

- -A <显示行数> --after-context=<显示行数> # 除了显示符合范本样式的那一行之外，并显示该行之后的内容。 

- b --byte-offset # 在显示符合范本样式的那一行之外，并显示该行之前的内容。 

- B<显示行数> --before-context=<显示行数> # 除了显示符合样式的那一行之外，并显示该行之前的内容。 

- c --count # 计算符合范本样式的列数。 

- C<显示行数> --context=<显示行数>或-<显示行数> # 除了显示符合范本样式的那一列之外，并显示该列之前后的内容。 

- -d<进行动作> --directories=<动作> # 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep命令将回报信息并停止动作。 

- -e<范本样式> --regexp=<范本样式> # 指定字符串作为查找文件内容的范本样式。 

- -E --extended-regexp # 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式。 

- -f<范本文件> --file=<规则文件> # 指定范本文件，其内容有一个或多个范本样式，grep查找符合范本条件的文件内容，格式为每一列的范本样式。 

- -F --fixed-regexp # 将范本样式视为固定字符串的列表。 

- -G --basic-regexp # 将范本样式视为普通的表示法来使用。 

- -h --no-filename # 在显示符合范本样式的那一列之前，不标示该列所属的文件名称。 

- -H --with-filename # 在显示符合范本样式的那一列之前，标示该列的文件名称。 

- -i --ignore-case # 忽略字符大小写的差别。 

- -l --file-with-matches # 列出文件内容符合指定的范本样式的文件名称。 

- -L --files-without-match # 列出文件内容不符合指定的范本样式的文件名称。 

- -n --line-number # 在显示符合范本样式的那一列之前，标示出该列的编号。 

- -q --quiet或--silent # 不显示任何信息。 

- -R/-r --recursive # 此参数的效果和指定“-d recurse”参数相同。 

- -s --no-messages # 不显示错误信息。 

- -v --revert-match # 反转查找。 

- -V --version # 显示版本信息。 

- -w --word-regexp # 只显示全字符合的列。 

- -x --line-regexp # 只显示全列符合的列。 

- -y # 此参数效果跟“-i”相同。 

- -o # 只输出文件中匹配到的部分。

## 常见用法

- 在文件中搜索一个单词，命令会返回一个包含 “match_pattern” 的文本行：
  
  ```shell
  grep match_pattern file_name 
  grep "match_pattern" file_name 
  grep "match_pattern" file_1 file_2 file_3 ... 
  ```

- 输出除match_pattern之外的所有行 -v 选项：
  
  ```shell
  grep -v "match_pattern" file_name
  ```

- 多个匹配，那么使用--作为各部分之间的分隔
  
  ```shell
  echo -e "a\nb\nc\na\nb\nc" | grep a -A 1
  ```

- 标记匹配颜色 --color=auto 选项：
  
  ```shell
  grep "match_pattern" file_name --color=auto
  ```

- 使用正则表达式 -E 选项：
  
  ```shell
  grep -E "[1-9]+" 
  或 
  egrep "[1-9]+" 
  ```

- 只输出文件中匹配到的部分 -o 选项：
  
  ```shell
  echo this is a test line. | grep -o -E "[a-z]+\." line. 
  echo this is a test line. | egrep -o "[a-z]+\." line. 
  ```

- 统计文件或者文本中包含匹配字符串的行数-c 选项：
  
  ```shell
  grep -c "text" file_name 
  ```

- 输出包含匹配字符串的行数 -n 选项
  
  ```shell
  grep "text" -n file_name 
  或 
  cat file_name | grep "text" -n 
  #多个文件 
  grep "text" -n file_1 file_2 
  ```

- 打印样式匹配所位于的字符或字节偏移：
  
  ```shell
  echo gun is not unix | grep -b -o "not" 7:not
  #一行中字符串的字符偏移是从该行的第一个字符开始计算，起始值为0。选项 **-b -o** 一般总是配合使用。 
  ```

- 搜索多个文件并查找匹配文本在哪些文件中：
  
  ```shell
  grep -l "text" file1 file2 file3... 
  ```

- grep递归搜索文件
  
  ```shell
  grep "text" . -r -n        # .表示当前目录。 
  ```

- 忽略匹配样式中的字符大小写：
  
  ```shell
  echo "hello world" | grep -i "HELLO" hello 
  ```

- 选项 -e 制动多个匹配样式：
  
  ```shell
  echo this is a text line | grep -e "is" -e "line" -o is line 
  #也可以使用 -f选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符。
  cat patfile aaa bbb echo aaa bbb ccc ddd eee | grep -f patfile -o 
  ```

- 在grep搜索结果中包括或者排除指定文件：
  
  ```shell
  #只在目录中所有的.php和.html文件中递归搜索字符"main()" 
  grep "main()" . -r --include *.{php,html}
  #在搜索结果中排除所有README文件
  grep "main()" . -r --exclude "README" 
  #在搜索结果中排除filelist文件列表里的文件
  grep "main()" . -r --exclude-from filelist 
  ```

- 使用0值字节后缀的grep与xargs：
  
  ```shell
  # 测试文件：
  echo "aaa" > file1 
  echo "bbb" > file2
  echo "aaa" > file3 
  grep "aaa" file* -lZ | xargs -0 rm 
   #执行后会删除file1和file3，grep输出用-Z选项来指定以0值字节作为终结符文件名（\0），xargs -0 读取输入并用0值字节终结符分隔文件名，然后删除匹配文件，-Z通常和-l结合使用。 
  ```

# Cut 切分文件

`cut` 命令从文件的每一行剪切字节、字符或字段并将这些字节、字符或字段写至标准输出。

如果不指定 `file` 参数， `cut` 命令将读取标准输入。 必须指定 `-b`、`-c` 或 `-f` 标志之一。

```shell
cut -f FIELD_LIST filename
# FIELD_LIST是需要显示的列。它由列号组成，彼此之间用逗号分隔。
 cut -f 2,3 filename
```

主要参数含义：

-  `-b`: 以字节为单位进行分割

- `-c`: 以字符为单位进行分割 

-  `-d`: 自定义分隔符进行分割，默认为制表符号

-  `-f`: 与 `-d` 一起使用，用分隔符分割后，指定显示哪些部分

-  `-n`：取消分割多字节字符，仅和 `-b` 标志一起使用

```bash
cut [-bn] [file] 或 
cut [-c] [file] 或 
cut [-df] [file]
```

## 常见用法

- 取得列
  
  ```shell
  # FIELD_LIST是需要显示的列。它由列号组成，彼此之间用逗号分隔。
  cut -f 2,3 filename
  ```

- 取得字段的位置
  
  | file | description                         |
  | ---- | ----------------------------------- |
  | dN-  | 从第N个字节、字符或字段开始到行尾                   |
  | N-M  | 从第N个字节、字符或字段开始到第M个（包括第M个在内）字节、字符或字段 |
  | -M   | 从第1个字节、字符或字段开始到第M个（包括第M个在内）字节、字符或字段 |
  
  ```shell
  cut -c2-5 range_fields.txt
  ```

# Sed

```shell
sed 命令行格式为： sed [选项] ‘command’ 输入文本
```

命令：

| 命令  | 作用        |
| --- | --------- |
| p   | 打印行       |
| d   | 删除行       |
| s   | 替换        |
| n   | 替换第几个匹内容  |
| w   | 另存为       |
| a   | 之后添加一行    |
| i   | 当前行之前插入文本 |
| y   | 替换匹配内容    |

## 常见用法

- 打印 `p`
  
  默认情况下，sed 把输入行打印在屏幕上，选项-n 用于取消默认打印操纵。当选项-n 和命令 p 同时出现时，sed 可打印选定的内容。
  
  ```shell
  sed '/north/p' sed.txt
  ```

- 删除 `d`
  
  命令 d 用于删除输入行。sed 先将输入行从文件复制到模式缓存区，然后对该行执行 sed命令，最后将模式缓存区的内容显示在屏幕上。如果发出的是命令 d，当前模式缓存区的输入行会被删除，不被显示。
  
  ```shell
  # 删除第 3 行。默认情况下，其余的行都被打印到屏幕上。
  sed '3d' sed.txt
  # 删除从第三行到最后一行内容，剩余各行被打印。
  sed '3,$d' sed.txt
  ```

- 替换 `s`
  
  命令 s 是替换命令。替换和取代文件中的文本可以通过 sed 中的 s 来实现，s 后包含在
  
  斜杠中的文本是正则表达式，后面跟着的是需要替换的文本。可以通过 g 标志对行进行全局替换
  
  ```shell
  # 全局替换
  sed 's/west/north/g' sed.txt
  
  # 选线-n 与命令行末尾的标志 p 结合，告诉 sed 只打印发生替换的那些行；也就是说，如果只有在行首找到 west 并替换成 north 时才会打印此行。
  sed -n 's/^west/north/p' sed.txt
  
  # 文件中出现的所有的 Hemenway 都被替换为 Jones，只有发生变化的行才会打印出来。选项-n 与命令 p 的组合取消了默认的输出。标志 g 的含义是表示在行内全局替换。
  sed -n 's/Hemenway/Jones/gp' sed.txt
  
  # 包含在圆括号里的模式 Mar 作为标签 1 保存在特定的寄存器中。替换串可以通过\1 来引用它。则 Margot 被替换为 Marlinane。
  sed 's/\(Mar\)got/\1linanne/p' sed.txt
  ```

```
- 指定行的范围：逗号

      行的范围从文件中的一个地址开始，在另一个地址结束。地址范围可以是行号（例如

5,10），正则表达式（例如/Dick/和/Joe/），或者两者的结合（例如/north/,$）范围是闭合的——包含开始条件的行，结束条件的行，以及两者之间的行。如果结束条件无法满足，就会一直操作到文件结尾。如果结束条件满足，则继续查找满足开始条件的位置，范围重新开始。

```shell
打印模式 west 和 east 之间所有的行。如果 west 出现在 east 之后的某一行，则打印的
范围从 west 所在行开始，到下一个出现 east 的行或文件的末尾（如果前者未出现）。
sed -n '/west/,/east/p' sed.txt

sed -n '5,/northeast/p' sed.txt

修改从模式 wast 和 east 之间的所有行，将各行的行尾($)替换为字符串**VACA**。换行
符被移到新的字符串后面。
sed '/west/,/east/s/$/**VACA**/' sed.txt
```

- 多重编辑 `e`
  
  -e 命令是编辑命令，用于 sed 执行多个编辑任务的情况下。在下一行开始编辑前，所有
  
  的编辑动作将应用到模式缓存区的行上。

```shell
说明：选项-e 用于进行多重编辑。第一重编辑编辑删除第 1~3 行。第二重编辑将
Hemenway 替换为 Jones。因为是逐行进行这两行编辑（即这两个命令都在模式空间的当前
行上执行），所以编辑命令的顺序会影响结果。例如，如果两条命令都执行的是替换，前一
次替换会影响后一次替换。

sed -e '1,3d' -e 's/Hemenway/Jones/' sed.txt
```

- 追加 `a`
  
  a 命令是追加命令，追加将新文本到文件中当前行(即读入模式的缓存区行)的后面。不
  
  管是在命令行中，还是在 sed 脚本中，a 命令总是在反斜杠的后面

```shell
命令 a 用于追加。字符串 Hello，World！被加在以 north 开头，north 后面跟一个空格
的各行之后。如果要追加的内容超过一行，则除最后一行外，其他各行都必须以反斜杠结尾。 
sed '/^north /a Hello,World!' sed.txt
```

- 插入 `i`
  
  i 命令是插入命令，类似于 a 命令，但不是在当前行后增加文本，而是在当前行前面插
  
  入新的文本，即刚读入缓存区模式的行

```shell
sed '/eastern/i Hello,World! \
```

- 修改命令 `c`
  
  c 命令是修改命令。sed 使用该命令将已有的文本修改成新的文本。旧文本被覆盖。

```shell
 sed '/eastern/c Hello,World! \
```

- 获取下一行 `n`
  
  n 命令表示下一条命令。sed 使用该命令获取输入文件的下一行，并将其读入到模式缓
  
  冲区中，任何 sed 命令都将应用到匹配行，紧接着的下一行上。

```shell
如果在某一行匹配到模式 eastern，n 命令就指示 sed 用下一个输入行（即包含 AM Main 
Jr 的那行）替换模式空间中的当前行，并用 Archie 替换 AM，然后打印该行，再继续往下处
理。
sed '/eastern/{n;s/AM/Archie/;}' sed.txt
```

- 转换 `i`
  
  y 命令表示转换。该命令与 tr 命令相似，字符按照一对一的方式从左到右进行转换。例
  
  如 y/abc/ABC/，会把小写字母转换成大写字母，a-->A,b-->B,c-->C

```shell
sed 
'1,3y/abcdefghijklmnopqrstuvwxyz/ABCDEFGHIJKLMNOPQRSTUVWXYZ/' sed.txt
```

- 退出 `q`
  
  q 命令表示退出命令。该命令将导致 sed 程序退出，且不再进行其他的处理。

```shell
sed '5q' sed.txt

在某行匹配到模式 Lewis 时，s 表示先用 Joseph 替换 Lewis，然后 q 命令让 sed 退出。
sed '/Lewis/{ s/Lewis/Joseph/;q; }' sed.txt
```

- 其他

```shell
移除空行
空行可以用正则表达式 ^$ 进行匹配。最后的/d告诉sed不执行替换操作，而是直接删除匹配到的空行：
sed '/^$/d' file

已匹配字符串标记
我们可以用&指代模式所匹配到的字符串，这样就能够在替换字符串时使用已匹配的内容.正则表达式\w\+匹配每一个单词，然后我们用[&]替换它
echo this is an example | sed 's/\w\+/[&]/g'


子串匹配标记
我们还可以使用\#来指代出现在括号中的部分正则表达式
echo this is digit 7 in a number | sed 's/digit \([0-9]\)/\1/'
echo seven EIGHT | sed 's/\([a-z]\+\) \([A-Z]\+\)/\2 \1/'
```

# AWK

awk命令可以处理数据流。它支持关联数组、递归函数、条件语句等功能。

```shell
awk 'BEGIN{ print "start" } pattern { commands } END{ print "end" }' file
awk [参数] [处理内容] [操作对象]

赋值：
awk 'BEGIN{a=5;a+=5;print a}'

逻辑运算符
awk 'BEGIN{a=1;b=2;print (a>2&&b>1,a=1||b>1)}'

正则：
awk 'BEGIN{a="100testaaa";if(a~/100/){print "ok"}}'
```

## 工作原理

(1) 首先执行BEGIN { commands } 语句块中的语句。

(2) 接着从文件或stdin中读取一行，如果能够匹配pattern，则执行随后的commands语句

块。重复这个过程，直到文件全部被读取完毕。

(3) 当读至输入流末尾时，执行END { commands } 语句块。

```shell
参数：
echo | awk '{ var1="v1"; var2="v2"; var3="v3"; \ 
 print var1,var2,var3 ; }'
```

## 内置变量

| 变量名      | 属 性                  |
| -------- | -------------------- |
| $0       | 当前记录                 |
| \$1 ~\$n | 当前记录的第 n 个字段         |
| FS       | 输入字段分隔符 默认是空格        |
| RS       | 输入记录分割符 默认为换行符       |
| NF       | 当前记录中的字段个数，就是有多少列    |
| NR       | 已经读出的记录数，就是行号，从 1 开始 |
| OFS      | 输出字段分隔符 默认也是空格       |
| ORS      | 输出的记录分隔符 默认为换行符      |

```shell
awd    -F'f' '{print $NF}' 1.txt

$0    : 代表当前行(相当于匹配所有)
awk -F: '{print $0, "---"}' /etc/passwd


$n    : 代表第n列

案例1:(以:为分隔符)
awk -F: '{print $1}' /etc/passwd

案例2:(默认空格为分隔符)
awk '{print $1}' /etc/passwd


NF    : 记录当前统计总字段数

案例1:(以:为分隔符 统计文件内每行内的行数)
awk -F: '{print NF}' /etc/passwd

案例2:(以:为分隔符 统计文件内每行总字段 并打印每行统计行数)
awk -F: '{print $NF}' /etc/passwd

NR    : 用来记录行号

案例1:
awk -F: '{print NR}' /etc/passwd

FS    : 指定文本内容分隔符(默认是空格)

案例1:
awk 'BEGIN{FS=":"}{print $NF, $1}' /etc/passwd

解析:
BEGIN{FS=":"}    : 相当于指定以 : 为分隔符
$NF            : 存储以 : 分隔符的最后一列
$1            : 存储以 : 分隔符的第一列
```

## 常见用法

- 将外部变量值传递给awk
  
  借助选项-v，我们可以将外部值（并非来自stdin）传递给awk：

```shell
VAR=10000
echo | awk -v VARIABLE=$VAR '{ print VARIABLE }'

var1="Variable1" ; var2="Variable2"
echo | awk '{ print v1,v2 }' v1=$var1 v2=$var2
```

- 用getline读取行
  
  awk默认读取文件中的所有行。如果只想读取某一行，可以使用getline函数。它可以用于在BEGIN语句块中读取文件的头部信息，然后在主语句块中处理余下的实际数据。

```shell
seq 5 | awk 'BEGIN { getline; print "Read ahead first line", $0 } 
{ print $0 }'
```

- 使用过滤模式对awk处理的行进行过滤

```shell
awk 'NR < 5' # 行号小于5的行
awk '/linux/' # 包含模式为linux的行（可以用正则表达式来指定模式）
awk '!/linux/' # 不包含模式为linux的行
awk 'NR==1,NR==4' # 行号在1到5之间的行
```

- 设置字段分隔符
  
  默认的字段分隔符是空格。我们也可以用选项-F指定不同的分隔符

```shell
awk -F: '{ print $NF }' /etc/passwd
awk 'BEGIN { FS=":" } { print $NF }' /etc/passwd
```

- 从awk中读取命令输出
  
  awk可以调用命令并读取输出。把命令放入引号中，然后利用管道将命令输出传入getline：
  
  "command" | getline output ;

# 
