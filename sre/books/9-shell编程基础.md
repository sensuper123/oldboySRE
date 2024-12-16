# 添加用户小脚本
```bash
name : useradd.sh
content :
#!/bin/bash
useradd test1
echo -e "\033[32m添加用户test\033[0m"
echo 1 | passwd --stdin &>/dev/null
echo -e "\033[33mpasswd:1\033[0m"
passwd -e test1 # 设置登录需要修改密码
```
# 显示系统信息脚本
```bash
初版，无变量引入
echo -e  "linux system version is \033[32m`cat /etc/centos-release`\033[0m"
echo -e "linux kencel is \033[32m`uname -r`\033[0m"
echo -e "cpu type name  is \033[32m `lscpu | grep 'Model name' |tr -s ' ' | cut -d':' -f2 `\033[0m"
echo -e "block is \033[32m `lsblk | grep '^sd' |tr -s ' ' |cut -d' ' -f1,4 |tr '\n' ' ' `\033[0m"
echo -e "mem size is \033[32m `free -h | grep 'Mem' |tr -s ' ' | cut -d' ' -f2 `\033[0m"

引入变量，优化
colorStart="\033[32m"
colorEnd="\033[0m"
把上述所有"\033[32m"替换成$colorStart,所有"\033[0m"替换$colorEnd
```
# 备份脚本
```bash
写一个备份脚本，/etc 备份到/backup/etcYear-MM-DD
#!/bin/bash
echo -e "\033[33m开始备份\033[0m"
sleep 2
date=`date +%F`
cp -av /etc /backup/etc$date
echo -e "\033[33m备份完成\033[0m"  
```
# 删除改移动脚本
```bash
#!/bin/bash
curenTime=`date +%F_%T`
dir=/tmp/curenTime
mkdir -p dir
mv $* dir
echo "rm is complited ,if you want replace ,pleace go to $dir"
```
# 磁盘大于80%脚本
```bash
#!/bin/bash
diskUsed=`df -h | egrep "root|sd*" |tr -s " " "%" |cut -d"%" -f5 |sort -rn | head -1`
[ $diskUsed -ge 80 ] && echo -e "\033[31m diskUsed > 80% \033[0m"|| echo -e "\033[32mdisk nomal\033[0m"
```
# 添加用户脚本
```bash
#!/bin/bash
[ $# -eq 0] && echo "需要用户名" && exit
id $1 >/dev/null && echo "用户已经存在" && exit
useradd $1 >/dev/null 2>&1 
echo 1 | passwd --stdin $1
```
# 普通变量和环境变量
```bash
普通变量定义
name=value
环境变量定义
export name=value
查看全部环境变量
env

普通变量只能在当前脚本或shell使用，脚本直接运行，会启动一个子shell，变量会与父shell进行隔离，即时在脚本内部定义了环境变量，也只是影响到子shell，而影响不到父shell
```
# shell 进程号查看
```bash
查看当前shell进程号
echo $BASHPID

查看父进程号
echo $PPID
```
# 小括号和花括号
```bash
1.小括号意味着启动一个子shell执行
NAME=lvyusen;echo $BASHPID;(echo $NAME ;echo $BASHPID;NAME=xiaolv);echo $NAME
32001
lvyusen
32183
lvyusen

2.花括号意味着在当前shell执行
NAME=lvyusen;echo $BASHPID; { echo $NAME; echo $BASHPID;NAME=xiaolv; } ;echo $NAME ;
32001
lvyusen
32001
xiaolv
```
# 位置定位
```bash
vim arg.sh

#!/bin/bash
echo "this is 1st $1" #第一个参数
echo "this is 2st $2"
echo "this is 10st ${10}"
echo "this is 11st ${11}"
echo "this is all $*" #所有参数，参数整合成一体
echo "this is all $@" #所有参数，参数一个一个的
echo "this is allNum $#" #参数个数
```
# shell简单运算
```bash
x=10
y=20
let z=x+y
z=$[x+y]
z=((x+y))
首选第一二种
例如显示系统信息脚本，可以把颜色改成随机的
colorStart="\033[$[RANDOM%7+31]"
```
# &&、||
	短路与
		前面为假，执行后面命令
		 前面为真，依旧执行后面命令
	 短路或
		 前面为真，结束执行
		 前面为假，执行后面命令

# test 
	作用：评估表达式，如果正确，例如 str1 = str2 ,则返回一个0，如果评估错误，则返回非0
```bash
val1=hello
val2=nihao
test $val1 = $val2 && echo "相同" || echo "不相同"

n=10
m=20
test $n -eq $m && echo "相等" || echo "不相等"
-eq : 相等
-ge : 大于等于
-gt : 大于
-le : 小于等于
-lt : 小于

除了test 用法，还可以使用[] 用法
比较两个数大小
[ "$n" -eq "$m" ] && echo "相等" || echo "不相等"

比较两个字符串是否相同
FILE1=file1；FILE2=file2;
[ "$FILE1" = "$FILE2" ] && echo "相同" || echo "不相同"

注意：在中括号内变量最好还是添加双引号，如果不添加，比较的时候，当变量为空时，会报格式错误，而如果添加了，尽管变量为空，当加了字符串，相当于比较空串。不会报格式错误

支持正则表达式
str="test.sh" | [[ str=~".sh$" ]] && echo ".sh"  || echo "not .sh"

两个判断
第一种，双中括号和单中括号判断，需要&&来连接两个判断
FILE=/lvyusen/test.sh; [[ "$FILE" =~ \.sh$ ]] && [ -x "$FILE" ] && $FILE

第二种，两个判断都可以写在单括号内，则用-a 、-o 来链接两个判断

FILE=/lvyusen/test.sh; [ -e "$FILE" -a -x "$FILE" ] && $FILE

```
# 取ip正则
```bash
egrep -o  "^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$"
```