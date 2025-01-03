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