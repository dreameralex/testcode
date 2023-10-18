# 基础

SHELL1 统计文件的行数

`wc -l` 是用来查看文件的newline的数量的。

```shell
方法1：
wc -l ./nowcoder.txt | awk '{print $1}'
方法2：
awk '{print NR}' ./nowcoder.txt |tail -n1
方法3：
awk 'END{print NR}' ./nowcoder.txt
方法4:
rep -c "" ./nowcoder.txt 
## 或者
$ grep -n "" ./nowcoder.txt  | awk -F ":" '{print $1 }' | tail -n 1
方法5：
sed -n '$=' ./nowcoder.txt
```

## SHELL7 打印字母数小于8的单词

```shell
#!/bin/bash
awk -F" " '{for(i=1;i<=NF;i++){if(length($i) < 8){print $i}}}' nowcoder.txt
```

用空格进行分割，NF是当前记录的字段数

## SHELL8 统计所有进程占用内存百分比的和

```shell
方法1：
sum=0
    for i in `awk '{print $6}' nowcoder.txt`
    do
        ((sum+=$i))
        done
    echo $sum
方法2：
awk '{sum+=$6}END{print sum}' nowcoder.txt

方法3：
sum=0;
while read p
do
    arr=($p)
    ((sum+=arr[5]))
done <nowcoder.txt
echo $sum
```

## SHELL9 统计每个单词出现的个数

```shell
cat nowcoder.txt | xargs -n1 | sort | uniq -c | sort -n | awk '{print $2, $1}'
```

## SHELL10 第二列是否有重复

```shell
方法1：
cat  $1 |awk '{print $2}'  |sort  |uniq -c|sort |grep -v 1

grep -v 为反向查找
```

## SHELL11 转置文件的内容

```shell
方法1：
awk '{
    for(i=1;i<=NF;i++){
      if(NR==1){
        row[i] = $i;
      }else{
        # 不是第一行时，将该行对应i列的值拼接到arr[i]
        row[i] = row[i]" "$i;
      }
    }
}END{
  for(i=1;i<=NF;i++){
    print row[i]
  }
}
' ./nowcoder.txt

NF 字段个数
NR 记录数（行号），从1开始，新的文件延续上面的计数，新文件不从1开始
```

数组拼接：

```shell
arr1=("apple" "banana")
arr2=("orange" "grape")
arr=("${arr1[@]}" "${arr2[@]}")
echo ${arr[@]} # 输出 apple banana orange grape
```

## SHELL12 打印每一行出现的数字个数

```shell
awk -F "[1,2,3,4,5]" #利用1-5作为分隔符将每行的字符串分隔开，统计每一行内容分割后的列数即可得到每行数字的个数
'BEGIN{sum=0}
    {
        print "line"NR" number:"(NF-1);
        sum+=(NF-1)}
END{print "sum is "sum}' nowcoder.txt
```

-F 指定输入文件折分隔符，fs是一个字符串或者是一个正则表达式，如-F:。

## SHELL13 去掉所有包含this的句子

```shell
方法1
grep -v 'this'

方法2
sed 命令 -> d 删除 -> // 包含要搜索的字符串
sed '/this/d'

方法3
awk 命令,检查当前 $0 不包含 this 随机输出
awk '$0!~/this/ {print $0}'
```

## SHELL14 求平均值

```shell
awk '{if(NR==1) {N=$1} else{sum+=$1}} END{printf ("%.3f",sum/N) }'
```

## SHELL15 去掉不需要的单词

```shell
grep  -E -v "[bB]"
```

- **-E 或 --extended-regexp** : 将样式为延伸的正则表达式来使用。

## SHELL16 判断输入的是否为IP地址

```shell
# 使用正则表达式
 awk '{
     if ($0 ~ /^((25[0-5]|2[0-4][0-9]|1[0-9][0-9]|[1-9][0-9]|[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1[09][0-9]|[1-9][0-9]|[0-9])$/) {
         print("yes");
     } else if ($0 ~ /[[:digit:]].[[:digit:]].[[:digit:]].[[:digit:]]/){
         print("no");
     } else {
         print("error")
     }
 }' nowcoder.txt

# 使用 . 作为分隔符
 awk -F '.' '{
     if (NF == 4) {
         for (i = 1; i < 5; i++) {
             if ($i > 255 || $i < 0) {
                 print("no")
                 break
             }
         }
         if (i == 5) {
             print("yes")
         } else {
             print("error")
         }
     }   
 }'

# bash 脚本，使用 . 作为分割符号
IFS='.'
while read line; do
    arr=(${line})
    if [ ${#arr[*]} -ne 4 ]; then
        printf "error\n"
    else
        for ((i = 0; i < ${#arr[*]}; i++)); do
            if [ ${arr[${i}]} -gt 255 ]; then
                printf "no\n"
                break
            fi
            done
        [ $i == 4 ] && printf "yes\n"
    fi
done
```

## SHELL17 将字段逆序输出文件的每行

```shell
awk -F ":" 
    '{
        a[NR]=$NF; 
        for (i=NF-1;i>0;i--) 
            a[NR]=a[NR]":"$i 
    }
    END{for(k in a) 
            print a[k]}' nowcoder.txt
```

## SHELL18 域名进行计数排序处理

```shell
 awk -F '/' '{print $3}' nowcoder.txt |sort|uniq -c|sort -r|awk '{print $1" "$2}'
```

## SHELL19 打印等腰三角形

```shell
for ((i=1;i<=n;i++))
do 
    #打印空格
    for ((j=n-i;j>=1;j--))
    do
        printf " "
    done
    #打印*
    for ((k=1;k<=i;k++))
    do
        printf "* "
    done
    #换行
    printf "\n"
done
```

## SHELL20 打印只有一个数字的行

```shell
awk -F"[0-9]" '{if(NF==2)print $0}'
```

## SHELL21 格式化输出

```shell
awk 'BEGIN{FS=""}
    {
        for(i=1;i<=NF;i++) 
        {
            if((NF-i)%3==0&&i!=NF) 
                printf $i",";
            else 
                printf $i};
        printf "\n"
    }' nowcoder.txt 这个判断算是个灵魂吧
```

## SHELL22 处理文本

```shell
awk -F ":" '{
        a[$1] = a[$1] $2 "\n"
    }
    END {for (i in a){
        printf("[%s]\n%s",i,a[i])
        }
    }' nowcoder.txt
```

```shell
cat nowcoder.txt | sort -t ":" -k 1,1 -sb | 
    awk -F ":" 
    '{if(1 != str)
        {
            print "["1"]\n"2;
            str=2;
            str =2;
            str=1;}
      else 
        {print $2
        }
    }'
```

## SHELL23 Nginx日志分析1-IP访问次数统计

```shell
grep "23/Apr/2020" nowcoder.txt | 
    awk '{print $1}' | sort | uniq -c | sort -r | awk '{print $1,$2}'
```

## SHELL24 Nginx日志分析2-统计某个时间段的IP访问量

```shell
grep "23/Apr/2020:2[0-3]" nowcoder.txt | awk '{print $1}' | sort | uniq 
```

## SHELL25 nginx日志分析3-统计访问3次以上的IP

```shell
awk '{print $1}' nowcoder.txt|sort|uniq -c|sort -nr|awk '($1>3){print $1,$2}'
```

## SHELL26 Nginx日志分析4-查询某个IP的详细访问情况

```shell
awk '$1~/192.168.1.22/{print $7}' nowcoder.txt|sort|uniq -c|awk '{print $1,$2}'
### 解析：
# awk '$1~/192.168.1.22/{print $7}' nowcoder.txt|sort|uniq -c 输出的是：
#    4 /1/index.php
#    2 /3/index.php
# 因为前面默认留了太多空格，所以再使用
# awk '{print $1,$2}'
# 格式化输出第1列、第2列
```

## SHELL27 nginx日志分析5-统计爬虫抓取404的次数

```shell
sed -n '/.*404.*baidu.*/p' nowcoder.txt | wc -l
```

利用wc指令我们可以计算文件的Byte数、字数、或是列数，若不指定文件名称、或是所给予的文件名为"-"，则wc指令会从标准输入设备读取数据。

-l或--lines 显示行数

p为打印

## SHELL28 Nginx日志分析6-统计每分钟的请求数

```shell
cat nowcoder.txt | awk '{print substr($4,14,5)}' | sort | uniq -c | sort -rn -k 1 | awk '{print $1,$2}'
```



## SHELL29 netstat练习1-查看各个状态的连接数

```shell
awk 'BEGIN{PROCINFO["sorted_in"]="@val_num_desc"}
    /tcp/{a[$6]++}
    END{for(v in a)print v,a[v]}' nowcoder.txt


grep tcp nowcoder.txt | awk '{print($6)}'  | sort | uniq -c | sort -t" " -k1nr | awk '{print($2,$1)}'
```



## SHELL30 netstat练习2-查看和3306端口建立的连接



## SHELL31 netstat练习3-输出每个IP的连接数

```shell
function solution_1() {
    grep tcp nowcoder.txt | awk -F":" '{print $2}' | awk '{print $2}' | sort | uniq -c | awk '{print $2,$1}' | sort -nr -k2
}
```

## SHELL32 netstat练习4-输出和3306端口建立连接总的各个链接数

```shell
awk '/tcp/{print $5}' nowcoder.txt |awk -F ':' '/3306/{print $1}'|sort|uniq -c|wc -l|awk '{print "TOTAL_IP "$1}'
awk  '/3306/{if($6=="ESTABLISHED"&&$1=="tcp")print $6}' nowcoder.txt|wc -l|awk '{print "ESTABLISHED "$1}'
awk '/3306/{if($1=="tcp")print $5}' nowcoder.txt|grep '3306'|wc -l|awk '{print "TOTAL_LINK "$1}'
```































































































# 命令

## xargs

xargs（英文全拼： eXtended ARGuments）是给命令传递参数的一个过滤器，也是组合多个命令的一个工具。

xargs 可以将管道或标准输入（stdin）数据转换成命令行参数，也能够从文件的输出中读取数据。

xargs 也可以将单行或多行文本输入转换为其他格式，例如多行变单行，单行变多行。

xargs 默认的命令是 echo，这意味着通过管道传递给 xargs 的输入将会包含换行和空白，不过通过 xargs 的处理，换行和空白将被空格取代。

xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令。

之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了 xargs 命令

- -a file 从文件中读入作为 stdin
- -e flag ，注意有的时候可能会是-E，flag必须是一个以空格分隔的标志，当xargs分析到含有flag这个标志的时候就停止。
- -p 当每次执行一个argument的时候询问一次用户。
- -n num 后面加次数，表示命令在执行的时候一次用的argument的个数，默认是用所有的。
- -t 表示先打印命令，然后再执行。
- -i 或者是-I，这得看linux支持了，将xargs的每项名称，一般是一行一行赋值给 {}，可以用 {} 代替。
- -r no-run-if-empty 当xargs的输入为空的时候则停止xargs，不用再去执行了。
- -s num 命令行的最大字符数，指的是 xargs 后面那个命令的最大命令行字符数。
- -L num 从标准输入一次读取 num 行送给 command 命令。
- -l 同 -L。
- -d delim 分隔符，默认的xargs分隔符是回车，argument的分隔符是空格，这里修改的是xargs的分隔符。
- -x exit的意思，主要是配合-s使用。。
- -P 修改最大的进程数，默认是1，为0时候为as many as it can ，这个例子我没有想到，应该平时都用不到的吧

### 单独使用

```shell
xargs # 等同于  
xargs echo

xargs find -name "*.txt"
上面的例子输入xargs find -name以后，命令行会等待用户输入所要搜索的文件。用户输入"*.txt"，表示搜索当前目录下的所有 TXT 文件，然后按下Ctrl d，表示输入结束。这时就相当执行find -name *.txt。
```

### -d 参数与分隔符

默认情况下，`xargs`将换行符和空格作为分隔符，把标准输入分解成一个个命令行参数。

```shell
echo "one two three" | xargs mkdir
```

上面代码中，`mkdir`会新建三个子目录，因为`xargs`将`one two three`分解成三个命令行参数，执行`mkdir one two three`。  
`-d`参数可以更改分隔符。

```shell
echo -e "a\tb\tc" | xargs -d "\t"
```

上面的命令指定制表符`\t`作为分隔符，所以`a\tb\tc`就转换成了三个命令行参数。`echo`命令的`-e`参数表示解释转义字符。

### -p 参数，-t 参数

使用`xargs`命令以后，由于存在转换参数过程，有时需要确认一下到底执行的是什么命令。  `-p`参数打印出要执行的命令，询问用户是否要执行。

`-t`参数则是打印出最终要执行的命令，然后直接执行，不需要用户确认。

### -0 参数与 find 命令

由于`xargs`默认将空格作为分隔符，所以不太适合处理文件名，因为文件名可能包含空格。  `find`命令有一个特别的参数`-print0`，指定输出的文件列表以`null`分隔。然后，`xargs`命令的`-0`参数表示用`null`当作分隔符。

```shell
 find /path -type f -print0 | xargs -0 rm
 查找文件，并以null分隔，xargs删除；
 find . -name "*.txt" | xargs grep "abc"
 查找txt文件，搜索其中包含"abc"的
```

### -L 参数

如果标准输入包含多行，`-L`参数指定多少行作为一个命令行参数。

```shell
xargs find -name "*.txt" "*.md" find: paths must precede expression: `*.md'
```

上面命令同时将`"*.txt"`和`*.md`两行作为命令行参数，传给`find`命令导致报错。  
使用`-L`参数，指定每行作为一个命令行参数，就不会报错。

```shell
 xargs -L 1 find -name "*.txt" ./foo.txt ./hello.txt "*.md" ./README.md
```

### -n参数

`-L`参数虽然解决了多行的问题，但是有时用户会在同一行输入多项。

```shell
xargs -n 1 find -name
echo {0..9} | xargs -n 2 echo

上面命令指定，每两个参数运行一次echo命令。所以，10个阿拉伯数字运行了五次echo命令，输出了五行。
```

### -I参数

如果`xargs`要将命令行参数传给多个命令，可以使用`-I`参数。  
`-I`指定每一项命令行参数的替代字符串。

```shell
$ cat foo.txt | xargs -I file sh -c 'echo file; mkdir file' 
```

上面代码中，`foo.txt`是一个三行的文本文件。我们希望对每一项命令行参数，执行两个命令（`echo`和`mkdir`），使用`-I file`表示`file`是命令行参数的替代字符串。执行命令时，具体的参数会替代掉`echo file; mkdir file`里面的两个`file`。

### --max-procs 参数

`xargs`默认只用一个进程执行命令。如果命令要执行多次，必须等上一次执行完，才能执行下一次。  
`--max-procs`参数指定同时用多少个进程并行执行命令。`--max-procs 2`表示同时最多使用两个进程，`--max-procs 0`表示不限制进程数。
