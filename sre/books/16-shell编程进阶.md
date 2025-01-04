# for in 使用循环创建用户
```bash
#!/bin/bash
for i in {1..10}
do
	#增加用户
	useradd user$i 
	#随机生成八位数密码
	password=`tr -dc [:alnum:]` < /dev/urandom | head -c 8
	#固定账号密码格式追加到文件
	echo user$i:$password | tee -a pass.txt
	#非交互式设置密码
	echo $password | passwd --stdin user$i
	#设置密码过期
	passwd -e user$i
	echo "user$i is created"
done
```
# for in 批量改名
```bash
#!/bin/bash
for fullName in `ls`
do
	name=`echo $fullName | sed -rn "s#(.*)\..*#\1#p"`
	mv $fullName $name.html
done
```
# for in 网络扫描
```bash
#实现测试主机能否ping通
#!/bin/bash
netId=192.168.1
for hostId in {0..254}
do
	{
	if ping -c1 -w1 netId.hostId ;then
		echo netId.HostId is up | tee hostStat.log
	}&
done
wait
```
# for in九九表
```bash
#!/bin/bash
color="\033["
for i in {0..9}
do
	for j in `seq 0 i`
	do
		echo -e "$color$[RANDOM%7+31]m${i}x$j=$[i*j] \033[0m \c\t"
	done
	echo 
done
```
# for 打印等腰三角形
```bash
#!/bin/bash
read -p "请输入行数:" LINE
for((i=1;i<=LINE;i++ ))
do
        for((j=0;j<LINE-i;j++))
        do
                echo -n " "
        done

        for((k=1;k<=2*i-1;k++))
        do
                echo -n "+"
        done
        echo
done

```
# for 随机生成十个数，输出最大值，最小值 
```bash
#!/bin/bash
max=$RANDOM
min=$max
for((i=0; i<9; i++))
do
	num=$RANDOM
	if((max < num)); then
		max=$num
	elif((num < minx)); then
		min=$num
	fi
done
echo max=$max , min=$min
```
# while 监控服务，关闭了自动重启
```bash
#!/bin/bash
while true
do
	skillall -0 httpd &>/dev/null
	if(($? > 0)); then
	systemctl restart httpd
	echo "At `date +%F_%T` httpd is restart" | tee -a /var/log/httdStart.log
	sleep 3
done
```
# while 菜单
```bash
#!/bin/bash
cat <<EOF
1.fotiaoqiang
2.mangtou
3.baicai
4.longxia
EOF
while true
do
read -p "请输入菜名编号:" NUM
case $NUM in
1)
        echo "fotiaoqiang price is \$20"
        ;;
2)
        echo "mantou price is \$2"
        ;;
3)
        echo "baicai price is \$5"
        ;;
4)
        echo "longxia price is $100"
        ;;
*)
        echo "please input num"
        ;;
esac
done

```
# while read 监控磁盘使用率报警
```bash
#!/bin/bash
df | sed -rn "/^\/dev/s#^([^[:space:]]+).* ([0-9]+)%.*#\1 \2#p" | while read diskName used
do
        if ((used >= 14)); then
                echo "$diskName is will full , used $used"
        fi
done
```
# select 生成菜单
```bash
#!/bin/bash
PS3="please input num: "
select MENU in fotiaoqiang mangtou baicai longxia quit
do
        case $REPLY in
        1)
         echo "$MENU price is \$20"
         ;;
        2)
         echo "$MENU price is \$2"
         ;;
        3)
         echo "$MENU price is \$5"
         ;;
        4)
         echo "$MENU price is $100"
         ;;
        5)
         break
         ;;
        *)
         echo "please input num"
         ;;
        esac
done
```
# function调用，实现score判断
```bash
functions file
	#函数实现判断是否数字
	func_isDigit(){
		if [[ $# eq 0]]; then
		 echo "not a digit"
		 return 10
		elif [[ $1 ~= ^[[:digit:]]+$ ]]; then
		 return true
		else
		 echo "please input a number"
		 return false
}

score.sh file
	source functions
	read -p "请输入你的成绩: " SCORE
	func_isDigit $SCORE
	if [ $? -ne 0  ]; then
	        exit
	else
	        if [ $SCORE -lt 60 ]; then
	         echo "so lower"
	        elif [ $SCORE -lt 80 ]; then
	         echo "nomoal"
	        else
	         echo "good"
	        fi
	fi

```
# recursive 求阶层
```bash 
#!/bin/bash
read -p "please input num: " NUM
fun_recursive(){
        if [ $1 -le 0 ]; then
         echo 1
        else
         local prev_num=$(fun_recursive $(($1-1)))
         echo $(($1 * prev_num))
        fi
}
fun_recursive $NUM
```
# trap 信号捕捉处理
```bash
trap "echo press ctrl + c" 2 # 捕捉ctrl + c 并修改操作
trap -p #打印使用了trap
for((i=0;i<10;i++))
do
	echo $i
	sleep 1
done

trap "" 2 # 捕捉ctrl + c 并修改操作,不作响应
for((i=10;i<20;i++))
do
	echo $i
	sleep 1
done

trap "-" 2 # 捕捉ctrl + c 并恢复默认操作
for((i=20;i<30;i++))
do
	echo $i
	sleep 1
done

```
# trap finish exit
```bash
执行完成或者中途退出都会执行finish函数，做收尾工作
finish(){
	echo finish is run
}
for((i=0;i<5;i++))
do
	echo $i
	sleep 1
done
```
# 数组赋值
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202501050136988.png?imageSlim)
# 数组引用
```bash
arryList[0]=wang
arryList[1]=zhang
echo ${arryList[0]} #wang

arryList=(1 2 3)
echo ${arryList[0]} #1

arryList1=([0]=1 [1]=2 [2]=3)
echo ${arryList[2]} #3

read -a arryList

declare -A menu
menu=([mantou]=￥2 [baicai]=￥4 [tufu]=￥5)
echo ${menu[mantou]} #￥2

```
# 数组 监控磁盘
```bash
#/bin/bash
declare -A arry_diskInfo
df | grep "^/dev" > disk.log
while read line
do
        index=`echo $line |  sed -rn "s#^([^[:space:]]+).*#\1#p"`
        arry_diskInfo[$index]=`echo $line | sed -rn "s#.* ([0-9]+)%.*#\1#p"`
        if (( ${arry_diskInfo[$index]} > 8 )); then
                echo $index will full used ${arry_diskInfo[$index]}
        fi
done < disk.log

```
# 数组 大小比较
```bash

#/bin/bash
declare -a arry_random
for((i=0;i<10;i++))
do
        arry_random[$i]=$RANDOM
        if ((i==0)); then
                MAX=${arry_random[$i]}
                MIN=$MAX
        else
        (( ${arry_random[$i]} > $MAX)) && { MAX=${arry_random[$i]};continue; }
        (( ${arry_random[$i]} < $MIN)) &&  MIN=${arry_random[$i]}
        fi
done
echo ${arry_random[*]}
echo MAX=$MAX MIN=$MIN

```
# expect 实现自动化
```bash
#!/bin/bash
ip=$1
user=$2
password=$3
expect <<EOF
set timeout 10
spawn ssh $user@$ip
expect {
"yes/no" { send "yes\n";exp_continue }
"password" { send "$password\n" }
}
expect "]#" { send "useradd hehe\n" }
expect "]#" { send "echo magedu |passwd --stdin hehe\n" }
expect "]#" { send "exit\n" }
expect eof
EOF
#./ssh5.sh 192.168.8.100 root magedu
```