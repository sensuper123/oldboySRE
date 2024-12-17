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