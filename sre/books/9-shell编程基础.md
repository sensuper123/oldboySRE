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
echo "this is all $*" #所有参数
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