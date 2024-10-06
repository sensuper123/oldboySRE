### linux 组成部分
1. 命令、软件服务
2. 命令解释器
3. 内核
个人理解概述：内核控制硬件，命令解释器控制内核，命令代表具体事假，例如关机命令，由命令解释器解释并控制内核去操作对应的硬件，从而达到关机目的。
## linux发行版本
1. 不同'版本'，其实主要是，内核、命令解释器、不同应用程序/桌面组成
2. Debian系列
	1. Debian系统(更新较慢，应用程序版本低)
	2. Ubuntu系统
3. 红帽系列
	1. RHEL红帽企业版（被IBM收购）
	2. CentOS（被红帽收购，做成红帽测试版）
4. 国产系列
	1. 中标麒麟（kylin v10）

## 安装CentOS 7 配置
1. 启动时按tab输入命令，主要作用是吧网卡名从ens33 变成 ethx
```javascript
	net.ifnames=0 biosdevname=0 
```
2.  网卡自动启动设置 ![image.png](https://s2.loli.net/2024/09/07/6j7z4wAi9uelbDJ.png)
3. IP设置 ![image.png](https://s2.loli.net/2024/09/07/d5s78NGoJZagAkC.png)
4. 虚拟网络编辑器配置 ![image.png](https://s2.loli.net/2024/09/07/dtgSVmXZA6k9vau.png)
