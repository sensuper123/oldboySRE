# 模块和文件路径
+ 模块路径
	+ /lib64/security/
+ 模块专用配置文件
	+ /etc/security/
+ 服务模块的配置文件
	+ cd /etc/pam.d/
## 服务模块配置文件格式
+ 模块类型
	+ auth 认证类型
	+ acount 账户限制
	+ password 修改密码复杂度检测
	+ session 记录监控会话期间进行的操作
+ 认证机制
	+ require 一票否决，进行这个模块后，还会进行接下来的模块认证，但结果已注定
	+ sufficient 一票通过，不必执行同一类型的其他模块
	+ options 模块可选，结果不重要
	+ include 调用其他配置文件中定义的配置信息
+ 模块
+ 参数
# 常见模块
+ pam_securetty.so
	+ 引用了该模块，只有在etc/secretty只有定义的终端名才能成功登录
	+ auth       required     pam_securetty.so
+ pam_shells.so
	+ 引用该模块，会检查用户shell类型是否在/etc/shells内
	+ auth       required     pam_shells.so
+ pam_nologin.so
	+ 如果/etc/nologin文件存在，则普通用户登录不进去
	+ account    required     pam_nologin.so
+  pam_limits.so
	+ 控制资源使用，也可用ulimit
	+ 该服务有特定的配置文件 /etc/security/limits.conf
	+ lvyusen       soft    nproc           20 # lvyusen用户最多能打开20个进程  