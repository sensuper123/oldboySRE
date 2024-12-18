	sed流编辑器，一次处理一行
	格式：sed [option] 'script' inputFile
	'script' 包含 位置 + 命令  
## options
+ -n 不自动打印
+ -f /path/file 从指定文件中读取编辑文本
+ -r 支持使用扩展正则
+ -i.bak 备份并处理原文件
# 位置可放内容
+ 单一行
	+ 直接放行号
		+ sed -n "2p" /etc/passwd
	+ 正则表达式 //
		+ ifconfig eth0 |  sed -n "/netmask/p"
+ 范围行
	+ 3 - 5行
		+ sed -n '3,5p' /etc/passwd
	+ 从 a开头 ，到l开头
		+ sed -n '/^a/,/^l/p' /etc/passwd
+ 步进
	+ 每隔两行就取一个
		+ seq  10 |  sed -n "1~2p"
# 命令可放内容
+ p 屏幕输出
+ d 删除
+ a text 在符合行后追加text内容
	+ seq 10 | sed "1~2a line"
	+ sed "/^root/a new line" passwd
+ i text 在符合行前追加text内容
+ c text 将符合行替换成text内容
	+ sed "/^SELINUX=/c SELINUX=disabled" /etc/selinux/config
+ w newfile 将操作后的文件另存为新文件(删除#号开头和空行)
	+ sed -r "/^#|^$/d ; w fstab.new" fstab
+ r /PATH/file 匹配到的行后读入另一个文件的内容
	+ 基数行后读入/etc/issue内容
	+ seq 10| sed  "1~2r /etc/issue"
+ = 将匹配行的行号打印出来，并打印该行内容
	+ sed -n "/root/{=;p}" /etc/passwd
	+ 加了花括号，表示匹配到模式后，两个命令一起执行
	+ 不加花括号，每个命令都是单独的
	+ 对于命令组合，除了d = p !不用参数的，其他都不能组合，例如{a  txx; p} 会直接替换成 txx;p ,而{p ; a txx} 会报语法错误
# 搜索替换
	前面都是操作行的，例如根据数字，根据模式过滤出对应行再根据命令进行删除或插入或替换整行，而如果要对对应字符进行操作，需要用到搜索替换
## sed取IP
```bash
ifconfig eth0 | sed -n "2s/^.*inet //;s/ netmask.*$//p"
ifconfig eth0 | sed -nr "2s/(^[^0-9]+)([0-9.]+)( .*$)/\2/p"
```

## 取出路径文件夹
```bash
取出/etc/sysconfig/network-scripts/的 dirname
echo  /etc/sysconfig/network-scripts/ | sed -r 's@(^/.*/)[^/]+/?$@\1@'
```
# 添加 net.ifnames=0
```bash
 sed -n '/linux16/s#.*#& net.ifnames=0#p' /boot/grub2/grub.cfg 
```
## 查找光盘不同架构有多少rpm包
```bash
[root@oldboy-study-lvyusen Packages]# ls *.rpm | sed -nr 's/.*\.(.*)\.rpm$/\1/p' | wc -l
```