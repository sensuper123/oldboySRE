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

# 管道和xargs
	判断是否需要xargs，简单点可以从两个方面入手
	第一：命令是否需要明确的参数集，例如文件名或者路径，如rm、cp、mv，这都需要明确的路径或者文件名，这时候就需要xargs
	 第二：命令是否支持标准输入，如果支持标准输入且要处理的内容就是标准输入的内容，就不需要xargs，例如grep，而如果要处理的是多个文件名，又回到了第一条规则，就需要xargs。