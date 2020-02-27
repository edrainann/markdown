# (04)常用Linux指令

### 1、查看日志

1) tail

```
tail -f xxx.log  #实时滚动
tail -100 xxx.log # 输出末尾100行

-f 循环读取
-q 不显示处理信息
-v 显示详细的处理信息
-c<数目> 显示的字节数
-n<行数> 显示行数
```

2) more/less

**"less is more"**

```
more只能进行向下搜索，less可以进行向上、向下搜索
more xxx.log #向下搜索log
less xxx.log #向上、下搜索log

/字符串：向下搜索“字符串”的功能
?字符串：向上搜索“字符串”的功能
n：重复前一个搜索（与 / 或 ? 有关）
N：反向重复前一个搜索（与 / 或 ? 有关）
= ：显示当前 行号
G ：移动到最后一行
g ：移动到第一行
行号数g ：可以跳转到该行号数，如12g，则跳转到第12行
b ：往前翻页
```

3)  grep - 数据查找定位

```
grep "ed" xxx.log # 查看xxx.log中包含ed字符的日志
grep -10 "ed" xxx.log # 查看匹配行的前后10行
grep -B 10 "ed" xxx.log # 查看匹配字符行的 前 10行
grep -A 10 "ed" xxx.log # 查看匹配字符行的 后 10行
grep -n "ed" xxx.log # 查看匹配字符行的 行号
```



### 2、网络

1）netstat - 得出ip、端口、pid

```
netstat -anp # 查看当前所有端口
netstat -tunlp # 查看当前所有与tcp、udp相关，状态为 LISTEN 的
-t (tcp) 仅显示tcp相关选项
-u (udp) 仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

2) ps

```
ps -ef # 可以查看进程的父进程ID和完整的COMMAND命令
ps aux  # 可以查看进程的CPU占用率和内存MEM占用率
```

3) kill

```
kill -l # 查看kill的所有信号
kill -9 pid # 强制终止，杀死进程
kill -15 pid # 终止进程（可以使得进程在退出之前清理并释放资源）
```



### 3、性能



### 4、数据处理

1) awk - 数据切片

```
awk 'pattern{action}' # 公式
awk 'BEGIN{}END{}' # 开始和结束
awk '/ed/' # 正则匹配
awk '/aa/,/bb/' # 区间选择
awk '$2~/xxx/' # 字段匹配
awk 'NR==2' # 取第二行
awk 'NR>1' # 去掉第一行
-F 或者 BEGIN{FS="_"} # 参数指定分隔符
BEGIN{RS="-"} # 参数记录分割符，单行变多行
$0 # 代表原来的行
$N # 代表第N个字段；如$2，则代表第二个字段
$NF #代表最后一个字段
#具体示例

[edrain ~]$echo '1:233:83_4' | awk 'BEGIN{FS=":";OFS="\n***";}{print $2,$NF}' # FS参数":"中的冒号为分隔符，ODF参数"\n***"中的参数为新分割符
233
***83_4
[edrain ~]$echo '1:233:83_4' | awk 'BEGIN{RS=":"}{print $1}';
1 #RS参数把单行变多行且成为一个整体
233
83_4
[edrain~]$echo '1:233:83_4' | awk 'BEGIN{RS=":";ORS="##";}{print $1}';
1##233##83_4## # ORS通过指定符合，把多行合并成一行

```



2) sed - 数据修改

```
 # 读取指定列
 cat -n xxx.log | sed -n "9,20p" # 仅列出xxx.log文件内的第9-20行
 nl -b a xxx.log | sed -n "9,20p" # 仅列出xxx.log文件内的第9-20行
 nl xxx.log | sed -n "9,20p" # 仅列出xxx.log文件内的第9-20行,但文件中的空白行不会加上行号
```





