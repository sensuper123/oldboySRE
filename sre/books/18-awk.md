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
# program
+ 支持算数运算符
+ 支持比较运算符
+ 支持逻辑运算符
+ 支持正则表达式
+ 支持三目运算符
+ 程序里面包含模式和动作，既然是程序，那就是在程序内可以使用的，都能使用
## parten格式
+ 什么都不写，代表每一行
+ // 使用正则
+ 关系表达式
```bash
# 取磁盘名和利用率
df | awk -F " +|%" '/^\/dev\//{print $1,$5}'
# 取ip地址
ip a | awk -F " +|/" '/inet /{print $3}'
# 取ss连接状态ip
ss -nt | awk -F " +|:" '/^ESTAB/{print $1, $(NF-2)}'
# 逻辑运算符或与比较运算符
awk -F ":" '$3>=100||$4<1000' /etc/passwd
# 行范围
awk "NR>=2 && NR <=6" /etc/passwd
# 模式匹配范围
awk "/^root/,/^bin/" /etc/passwd
```
## action
+ print printf
+ while
+ if
+ for
+ break
+ continue
```bash
# awk + if 取磁盘利用率
df | awk -F " +|%" '/^\/dev\//{if($5>=20){print $1"will be fulled","used is" $5  }else {print $1"is normal"}}'
# awk + for 统计单词个数
awk -F" " '/linux16/{for(i=1;i<NF;i++){print $i,length($i)}}' /etc/grub2.cfg
# awk + for 累加
awk 'BEGIN{sum=0; for(i=0;i<=100;i++){sum+=i};print "sum="sum}'
# awk 数组遍历
awk'BEGIN{arry["ceo"]="ceo1";arry["cto"]="cto1";arry["coo"]="coo1";for(v in arry){print v ,arry[v]}}'
# 统计ip地址出现次数
awk '{ip[$0]++}END{for( v in ip){print v , ip[v]}}' ip.txt | sort -k2 -nr | head -3

```