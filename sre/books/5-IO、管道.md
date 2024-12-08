# IO设备
```bash
每个进程都会有标准输入，标准输出，标准错误，对应的号码为 0 ， 1 ，2
默认标准输入是键盘
标准输出是终端
```
# 改变默认IO
```bash
hostname > filename 
command > filename.true 2 > filename.error
command &> filename.all 所有输出重定向
command > filename.all 2 > &1 
command 2>filename.all 1>&2

多行文本创建文件
cat <<EFO>>filename

```
# tr小工具
![image.png](https://lvyusen-1316126434.cos.ap-guangzhou.myqcloud.com/images/202412070528332.png?imageSlim)
```bash
tr -t abcd 12345
tr  [:space:] '\n' < f1.txt
```
# 管道
```bash
将前一个命令的处理值传输给后一个命令
判断什么时候需要xargs
xargs主要功能
	1.将传输过来的管道文本转换为参数集
	2.可控制后一个命令执行时间，等待管道传值后再执行
```