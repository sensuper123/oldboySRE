# 四剑客所擅长
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202411050125177.png?imageSlim)
# 格式
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202411050136334.png?imageSlim)
# 取行
```bash
1.取出etc/passwd的第一行
awk 'NR==1{print $0}' /etc/passwd
awk 'NR==1' /etc/passwd
NR : number of record 行号
$0 : 当前行内容

2.取出2-5行
awk 'NR >= 2 && NR <= 5' /etc/passwd

3.过滤出/etc/passwd 包含root或nobody的行
awk '/root | nobody/' /etc/passwd

4.过滤从root到nobody的行
awk '/root/,/nobody/' /etc/passwd
```
# 取列
```bash
1.使用awk取出ls -lh的大小列和最后一列
ls -lh /etc/| awk '{print $5,$NF}'

2.使用column -t 可对齐
ls -lh /etc/ | awk '{print $5,$NF}' | column -t

3.取出/etc/passwd中的第1列，第3列和最后一列
awk -F':' '{print $1,$3,$NF}' /etc/passwd | column -t 

4.取出 ip address show eth0 的ip
ip address show eth0 | awk 'NR==3{print $2}' | awk -F'/' '{print $1}'
ip address show eth0 | awk -F'[ /]+' 'NR==3{print $3}'
ip address show eth0 | awk -F'inet |/24' 'NR==3{print $2}'
```
# 取行取列
```bash
1.取行取列取ip地址
ip address show eth0 | awk -F'[ /]+' 'NR==3{print $3}'

2.取出权限部分 stat /etc/hosts
stat /etc/hosts | awk -F'[(/]' 'NR==4{print $2}'

3.取出/etc/passwd 第三列>100 的行的第1列，第3列和最后一列
awk -F':' '$3>100{print $1,$3,$NF}' /etc/passwd

4.当swap使用超过0则输出“异常系统开始占用wasp”
free | awk '/Swap/ && $3 >= 0{print "异常系统开始占用swap"}'

5.过滤出/etc/passwd第4列的数字是以0或1开头的行，输出第1列，第3列
awk -F':' '$4 ~ /^[01]/{print $1,$3}' /etc/passwd | column -t
~ ： 包含 $4 ~ /^[01]/ ：第四行开头包含0或1
```
# 统计计算
```bash
1.统计行数
awk '{i++} END{print i}' /etc/passwd

2.统计第四列包含0,1的行数
awk -F: '$4 ~ /^[01]/ {i++} END{print i}' /etc/passwd

3.计算第四列所有0,1开头的总和
awk -F: '$4 ~ /^[01]/ {sum += $4} END{print sum}' /etc/passwd
```