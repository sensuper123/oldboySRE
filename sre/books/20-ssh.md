# 基于key-expect，同时设置多台主机的ssh脚本
```bash
# 主机ip文件
echo 10.0.0.210 >> hostIp.txt
#脚本文件
vim ssh_expect.sh
#!/bin/bash
#生成用户私钥
ssh-keygen -t rsa -P "" -f ~/.ssh/id_rsa &>/dev/null && echo "ssh key is created"
# 检查是否安装expect
rpm -q expect &>/dev/null || yum -y install expect &>/dev/null
# 循环复制
while read IP 
do
expect <<EOF
set timeout 20
spawn ssh-copy-id -i /root/.ssh/id_rsa.pub $IP
expect {
"yes/no" {send "yes\n";exp_continue}
"password" {send "$PASS\n"}
}
expect eof
EOF
echo "$IP is ready"
done < hostIp.txt

```
# rsync 远程复制
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202501160123831.png?imageSlim)
	rsync -av /data/f* 10.0.0.210:/data/
# pssh
## 选项
+ -h : host 文件
+ -H : 单个主机字符串
+ -A : 基于密码管理
+ -i : 每个服务器内部处理信息输出
+ -o : 执行结果存放到文件
```bash
# 批量已配置key认证主机的selinux
pssh -h hostIp.txt -i "sed -i 's#SELINUX=disable#SELINUX=enforcing#' /etc/selinux/config"
```
## pscp.pssh 批量复制本机文件到远程
	pscp.pssh -h hostIp.txt /etc/fstab /data
## pslurp 批量复制远程主机文件到本机
+ -L ： local，存储到本机目录
+ -r ：文件夹递归
```bash
#把远程主机的/etc/passwd 复制到本机 /data 并改名user
pslurp -h hostIp.txt -L /data /etc/passwd user
```
## ssh端口转发
	外网主机连接内网服务，通过内网ssh跳板机，与外网主机建立隧道，将通信内容加密，ssh跳板机监听隧道请求，之后把数据发给目标服务器
```bash
1.telnet 服务器配置
yum install -y 
#确保/etc/securetty 有pts/0 pts/1 pts/2
#模拟外网拒绝telnet服务
iptables -A INPUT -s 10.0.0.200 -j REJECT 

2.外网客户端配置
# 开启监听9527端口，目标机器  跳板机器 -f：后台运行 N：不登录到跳板机
#客户端监听9527、后把数据封装成ssh协议的包，再发给跳板机，跳板机再发给目标服务器 
ssh -L 9527:10.0.0.220:23 10.0.0.210 -fN
# 把数据发到9527端口，ssh会把数据封装后发给跳板机
telnet 127.0.0.1 9527
```

```bash
smtp 邮件服务
1.smtp服务器配置
#监听本机修改成监听所有机器
vim /etc/postfix/main.cf 
systemctl restart postfix.service 

2.外网客户端配置
# 开启监听8888端口，目标机器  跳板机器 -f：后台运行 N：不登录到跳板机
#客户端监听8888、后把数据封装成ssh协议的包，再发给跳板机，跳板机再发给目标服务器 
ssh -L 8888:10.0.0.220:25 10.0.0.210 -fN
telnet 127.0.0.1 8888
hello a.com #打招呼
mail form : lvyusen@qq.com #发件人
rcpt to: root #收件人root
data #内容
subject :test2 #标题
how are you 
. #正文结束
quit #退出
```
## 远程端口转发(内网往外网建立隧道)
```bash
1.内网跳板机配置
# 打开外网远程端口9999监听ssh数据，至此建立外网和内网的隧道
ssh -R 9999:10.0.0.220:80 10.0.0.200 -fN

2.外网访问 9999，会把数据ssh封装发给内网跳板机

数据传输情况
data > 外网ssh server:9527 > ssh server:22 > localhost:xxx(内网跳板机)>telnet>telnet server(目标服务)

```
## 代理服务器上网
```bash
1.本机配置，建立隧道
ssh -D 9527:10.0.0.210 -fN
2.访问网站
curl --socks5 127.0.0.1:9527 http://10.0.0.220

# 实现window上网，10.0.0.200作为window的代理，加了g后，监听9527这个端口就不止本机能访问，而是所有ip都能访问
方法一：window > 10.0.0.200 > 10.0.0.210 > 10.0.0.220 目标网站
ssh -D 9527:10.0.0.210 -fNg
#模拟目标主机拒绝
iptables -A INPUT -s 192.168.1.3 -p tcp --dport 80 -j REJECT

方法二：
#直接把10.0.0.210即设置成window代理，也设置成ssh-server(ssh客户端服务器一体)
ssh -D 10.0.0.210 -fNg
```
# ssh服务器配置
## ssh优化
+ UseDNS no
+ GSSAPIAuthentication no
## dropbear 和 文件完整性监控aide 22-04