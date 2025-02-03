# dns只缓存服务器实现
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
# dns主服务器实现
```bash
1. 配置文件选项
zone "." IN {
        type hint;
        file "named.ca";
};

域的概念：.代表根域(需要解析的域名),file是存放域的数据库，type hint是根域，比较特殊。如果是主服务器，type是master

db数据库的格式
name ttl(time to live) IN rr_type value
name : 要解析的域名
ttle : 缓存存活时间
IN   : 目前不写
rr_type: 
	名字 -> ipv4 : A
	名字 -> ipv6 : AAAA
	ip   -> 名字 : ptr
	SOA : dns数据库的第一条信息，存放域的描述信息，例如该域由哪个dns提供服务


db数据库的第一条内容
$TTL 1D         (主dns服务器名称)    (管理者邮箱)
@       IN SOA  master.lvyusen.com. admin.lvyusen.com (
                                0       ; serial #版本
                                1D      ; refresh #同步频率
                                1H      ; retry # 1天同步失败，再次尝试
			                    1W      ; expire #一周无法同步 失效
                                3H )    ; minimum #不存在记录缓存时长
        NS      master # 可能有多个dns服务器为这个域名服务
master A 10.0.0.200 #主服务器指向本机
ftp    A 1.1.1.1 # ftp主机记录
http   A 2.2.2.2 # http主机记录
www    CNAME websrv #别名
websrv A 10.0.0.210
websrv A 10.0.0.220 #可对应多个ip

@ : 代表域名 => lvyusen.com.
SOA : 第一条数据
master.lvyusen.com. : 主服务器
admin.lvyusen.com : dns管理者邮箱


修改配置文件/etc/named.conf
zone "lvyusen.com." IN {
        type master;
        file "lvyusen.com.zone";
};

检查配置文件格式是否正确
named-checkconf 

检查db数据库格式是否正确
named-checkzone lvyusen.com lvyusen.com.zone
```
# 反向解析配置
```bash
1.反向解析定义域
vim /etc/named.rfc1912.zones
	zone "0.0.10.in-addr.arpa" IN {
	type master;
	file "10.0.0.zone";
	};
2.反向解析域配置
vim /var/named/10.0.0.zone
	$TTL 1D
	@	SOA master.lvyusen.com. admin.lvyusen.com. (1 1D 1H 1W 1H )
	NS master
	master	A	10.0.0.200
	15	PTR	15.lvusen.com
	16	PTR	16.lvyusen.com
	200	PTR	master.lvyusen.com

3.测试机测试
	dig -x 10.0.0.200 @10.0.0.200
```
# 主从服务器复制
```bash
1. 从服务器配置
	1. yum install -y bind
	2. vim /etc/named.conf
		1. //      listen-on-v6 port 53 { ::1; };
		2. //      allow-query     { localhost; };
	3. vim /etc/
		zone "lvyusen.com" IN {
        type slave;
        masters {10.0.0.200;};
        file "slaves/lvyusen.com.zone.slave"
	};
		allow-transfer {none;};


2.主服务器配置
	1.vim /var/named/lvyusen.com.zone 添加从服务器
		NS      NS2
		NS2   A 10.0.0.210
	2.设置服务器可做为从服务器
	vim /etc/named.conf
		allow-transfer {10.0.0.210;};
	
```