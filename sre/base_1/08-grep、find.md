## grep
### grep作用
1. 过滤：在文件中或管道中进行查找，找出想要的内容，默认按照行查找
2. grep会把匹配到的行显示出来
### 选项
```bash
-n : line-number 显示行号
-i : ignore-case 忽略大小写
-v : 排除，取反
```
### 案例
```bash
1.基本用法
[root@oldboy-study-lys ~]# grep 'root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

2.接管道
[root@oldboy-study-lys ~]# cat /etc/passwd | grep 'root'
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

3.显示行号

[root@oldboy-study-lys ~]# grep -n 'root' /etc/passwd
1:root:x:0:0:root:/root:/bin/bash
10:operator:x:11:0:operator:/root:/sbin/nologin

4.忽略大小写
[root@oldboy-study-lys ~]# grep  'Root' /etc/passwd //无匹配
[root@oldboy-study-lys ~]# grep -i 'Root' /etc/passwd
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin

5.排除
[root@oldboy-study-lys ~]# grep -v 'root' /etc/passwd | wc -l
19

```
## find
### 作用
	在指定文件夹中寻找文件
### 选项
```bash
-type : 指定文件类型 , f表示文件，d表示目录
-name : 指定文件名
-size : 根据大小查找文件 +表示大于 -表示小于
-mtime : 根据修改时间查找文件
-maxdepth : 指定查找最多层数
-iname : 不区分大小写
```
### 案例
```bash
1.精确查找
[root@oldboy-study-lys ~]# find  /etc -type f -name 'hostname'
/etc/hostname

2.模糊查找
[root@oldboy-study-lys ~]# find /etc/ -type f -name '*.conf'
/etc/resolv.conf ......

3.根据文件大小查找
[root@oldboy-study-lys ~]# find /etc/ -type f -size +500k
/etc/selinux/targeted/active/policy.kern
/etc/selinux/targeted/contexts/files/file_contexts.bin

4.根据修改时间查找
[root@oldboy-study-lys ~]# find /etc -type f -mtime +5379
/etc/pki/nssdb/cert8.db
[root@oldboy-study-lys ~]# ll /etc/pki/nssdb/cert8.db 
-rw-r--r--. 1 root root 65536 1月  13 2010 /etc/pki/nssdb/cert8.db

5.综合案例
找出/etc/中以.conf结尾大于10kb修改时间是7天之前的文件
[root@oldboy-study-lys ~]# find /etc/ -type f -name '*.conf' -size +10k -mtime +7
/etc/lvm/lvm.conf

6.指定查找层数
[root@oldboy-study-lys ~]# find /etc/ -maxdepth 1 -type f -name '*.conf'
/etc/resolv.conf

7.查找时不区分文件名大小写
[root@oldboy-study-lys ~]# find /etc/ -maxdepth 1  -type f -iname '*.CONF' 
/etc/resolv.conf

8.找出/lvyusen/find 以.txt结尾的文件并显示文件信息且统计条数
方法一：
[root@oldboy-study-lys find]# ll -h `find /lvyusen/find/ -type f -name '*.txt'` | wc -l //10
方法二：
[root@oldboy-study-lys find]# find /lvyusen/find/ -type f -name '*.txt' | xargs ls -lh 

```
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202410070343113.png?imageSlim)


