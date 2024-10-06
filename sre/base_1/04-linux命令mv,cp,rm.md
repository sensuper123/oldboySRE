## mv移动命令
```shell
	--移动
	mv /lvyusen/lvyusen.txt  /tmp/
	--改名
	mv /tmp/lvyusen.txt /tmp/lvyusen996.txt
```
## cp复制命令
![image.png](https://s2.loli.net/2024/09/10/BAHslUFOwcipIv4.png)
## rm 删除命令
```shell
	rm - f 强制删除不提示
	rm - r  递归删除
```
## echo 输出命令
```shell
	创建lvyusen.txt , 内容为woaini
	echo woaini > /lv
	yusen/lvyusen.txt
	
	> 重定向符号，清空内容再写入
	>> 追加重定向，在后面追加内容
	
	{} 输出有规律的内容
	[root@oldboy-study-lvyusen ~]# echo l{01..10}
	l01 l02 l03 l04 l05 l06 l07 l08 l09 l10
```
## cat显示文件内容
```shell
	cat -n etc/password -n:显示行号

	合并然后保存在all.txt
	cat /etc/hostname /etc/password > /lvyusen/all.txt 
```
## vi,vim编辑器
```sheel
	
```