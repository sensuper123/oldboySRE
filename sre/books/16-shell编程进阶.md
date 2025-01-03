# 使用循环创建用户
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
# 批量改名
```bash
#!/bin/bash
for fullName in `ls`
do
	name=`echo $fullName | sed -rn "s#(.*)\..*#\1#p"`
	mv $fullName $name.html
done
```
# 网络扫描
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
# 九九表
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
# 打印等腰三角形
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
# 随机生成十个数，输出最大值，最小值
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
# 监控服务，关闭了自动重启
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
# 菜单
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
# 监控磁盘使用率报警while read
```bash
#!/bin/bash
df | sed -rn "/^\/dev/s#^([^[:space:]]+).* ([0-9]+)%.*#\1 \2#p" | while read diskName used
do
        if ((used >= 14)); then
                echo "$diskName is will full , used $used"
        fi
done
```
# select生成菜单
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
# 求阶层
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