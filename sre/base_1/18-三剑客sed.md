# 概述
1. 取行，过滤，替换修改文件内容
2. 后向引用(截取)
# 格式
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202411030151815.png?imageSlim)
![](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202411030422249.png?imageSlim)
# 查找
```bash
1.取出文件的第三行
sed -n '3p' /etc/passwd

2.取出文件3-5行
sed -n '3,5p' /etc/passwd

3.取出第1行，第5行
sed -n '1p;5p' /etc/passwd

4.取出包含root的行
sed -n '/^root/p' /etc/passwd

5.根据范围取出对应行
sed -n '/102/,/105/p' /lvyusen/log
```
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202411030306051.png?imageSlim)
# 修改
## 普通
```bash
1.把/lvyusen/log 的lvyusen替换成lodyusen
sed -i 's#lvyusen#oldyuse#g' /lvyusen/log

2.进行替换前先备份
sed -i.bak 's#lvyusen#oldyuse#g' /lvyusen/log
```
## 进阶
```bash
1.把12345678变成1<234567>8
echo 12345678 | sed -r 's#(1)(.*)(8)#\1<\2>\3#g'

2.交换etc/passwd第一列和最后一列的内容
sed -r 's#(^.*)(:x.*:)(.*)#\3\2\1  #g' /lvyusen/passwd

3.取ip address show etho 的ip地址
 ip address show eth0 | sed -n '3p' | sed -r 's#(.*et )(.*)(/.*)#\2#g'

4.取出stat /etc/hosts中的0644或644
0644
stat /etc/hosts | sed -n '4p'|sed -r 's#.*\(([0-9]+)/.*#\1#g'
644
stat /etc/hosts | sed -n '4p'|sed -r 's#.*\([0-9]([0-9]+)/.*#\1#g'


```
# 删除
1. d 删除行
2. 删除行内内容添加用 s###g 替换
```bash
1.用sed实现排除空行
sed -r '/^$|#/d' /etc/ssh/sshd_config

2.grep方法实现排除空行
egrep -v '^$|#' /etc/ssh/sshd_config

3.awk实现
awk '!/^$|#/' /etc/ssh/sshd_config
```
# 增加
+ cai
	+ a : append 指定行后追加
	+ i : insert 指定行号前插入
	+ c : replace 指定行号替换
```bash
在log文件第三行后追加999，lvyusen
sed '3a 999,lvyusen' /lvyusen/log
```
# 习题
```bash
1.过滤出etc/passwd包含root|nobody的行
egrep 'root|nobody' /etc/passwd
sed -nr '/root|nobody/p' /etc/passwd
sed -r '/root|nobody/!d' /etc/passwd
awk '/root|nobody/' /etc/passwd

2.过滤出etc/passwd以root开头的行
egrep '^root' /etc/passwd
sed -rn '/^root/p' /etc/passwd
sed -r '/^root/!d' /etc/passwd
awk '^root' /etc/passwd

3.过滤出/etc/ssh/sshd_config中包含permitrootlogin或usedns的行
egrep -i 'permitrootlogin|usedns' /etc/ssh/sshd_config
sed -rn '/permitrootlogin|usedns/Ip' /etc/ssh/sshd_config
sed -r '/permitrootlogin|usedns/I!d' /etc/ssh/sshd_config

4.取出etc/passwd以bash结尾的行
egrep 'bash$' /etc/passwd
sed -rn '/bash$/p' /etc/passwd
sed -r '/bash$/!d' /etc/passwd
awk '/bash$/' /etc/passwd

5.显示/etc/下面一层中以.conf结尾的文件
find /etc/ -type f -name *.conf -maxdepth 1
ls /etc/ | egrep '\.conf$'
ls /etc/ | sed -n '/\.conf$/p' 
ls /etc/ | awk '/\.conf$/'

6.使用grep取出/etc/passwd第一列数据也就是用户名
egrep -o '^[0-Z_-]+' /etc/passwd
awk -F: '{print $1}' /etc/passwd
egrep -o '^[^:]+' /etc/passwd
sed -r 's#(.*):x.*#\1#g' /etc/passwd
sed -r 's#:.*##g' /etc/passwd

7.把上题结果写入/lvyusen/name.txt
egrep -o '^[0-Z_-]+' /etc/passwd >/lvyusen/name.txt

8.找出文件3-5行，最后一行
sed -n '3,5p' /lvyusen/log
awk 'NR>=1 && NR<=3' /lvyusen/log
head -3 /lvyusen/log
最后一行
tail -1 /lvyusen/log 
sed -n '$p' /lvyusen/log
awk 'END{print}' /lvyusen/log

9.找出/etc/下面所有的以.conf结尾的文件，然后过滤包含linux的行
find+grep:
find /etc/ -type f -name '*.conf' |xargs grep 'linux'
grep 'linux' `find /etc/ -type f -name '*.conf'`
find /etc/ -type f -name '*.conf' -exec  grep 'linux' {} \;

find+sed:
find /etc/ -type f -name '*.conf' |xargs sed -n '/linux/p'
sed -n '/linux/p' `find /etc/ -type f -name '*.conf'`
find /etc/ -type f -name '*.conf' -exec  sed -n '/linux/p' {} \;

find+awk:
find /etc/ -type f -name '*.conf' |xargs awk '/linux/'
awk  '/linux/' `find /etc/ -type f -name '*.conf'`
find /etc/ -type f -name '*.conf' -exec  awk  '/linux/' {} \;

10.取出etc/passwd中每个字符或单词，统计次数出现最多的
字符：
grep  -o '.' /etc/passwd | sort |uniq -c | sort -nr| head -5
单词：
egrep  -o '[a-Z]+' /etc/passwd | sort |uniq -c | sort -nr| head -5

```