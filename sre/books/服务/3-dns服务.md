```bash
#安装bind
yum install -y bind
# 启动服务
systemctl start named.server
# 配置文件
/etc/named.cfg
#数据存放路径
/var/named
#日志
/var/log/named.log

# 询问方式dns服务器配置
listen-on port 53 { localhost; }; #监听本机所有ip
allow-query     { any; }; #允许外部机器询问
``` 