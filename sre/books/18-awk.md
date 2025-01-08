# awk变量
+ NF 第几列 NF-1最后第二列
+ NR 行数 NR== 2 第二行
+ FS 变量指定分隔符
+ OFS 变量指定输出分隔符
+ RS 变量指定记录符，默认换行符
+ ORS 变量指定输出记录符
# printf
+ %s
+ %d
+ %f
+ %-10s 字符长度10个单位，且左对齐
```bash
# 输出添加分隔符+++
awk -F ":" '{print $1,"+++",$3}' /etc/passwd
# 处理第二行
awk -F: 'NR==2{print $1,$3}' /etc/passwd
# 以变量形式指定分隔符
awk -v FS=":" '{print $1,FS,$3}' /etc/passwd
# 以变量形式指定分隔符，输出分隔符
awk -v FS=":" -v  OFS="+++" '{print $1 OFS $3}' /etc/passwd
# printf format 输出
awk -F ":" '{printf "username:%-20s  UID:%-10d", $1 $3}' /etc/passwd



```