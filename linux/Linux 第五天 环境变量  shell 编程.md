## Linux 第五天 环境变量  shell 编程    

## 环境变量  

## 全局变量  

 ```
常见的全局环境变量   
  PATH 指令的搜索路径
  HOME 用户的家目录
  LOGNAME 登录名
  SHELL 脚本的类型 
  
 使用全局环境变量 
 echo $PATH
 
 自定义全局变量  
  name='kangbazi'
  echo $name;
  
 配置系统环境变量 
 vim /etc/profile
 export name=1804 
 source /etc/profile
 
 
 echo $name 
 ```

### 设置path环境变量   

```
echo  $PATH 

1.对所有的用户生效   
 sudo vim /etc/profile

 export PATH=$PATH:/usr/local/mysql/bin   给 mysql 添加 path环境变量  类似于windows 中 
 此电脑->右键属性->高级系统设置->环境变量 ->系统环境变量->path 
 
  source /etc/profile 让配置文件立即生效 
2.对登录的用户有效  
   sudo vim /root/.bashrc 
   	 export PATH=$PATH:/usr/local/mysql/bin  
   sudo source /root/.bashrc 

```

## shell编程   

> 命令解释器  将用户输入的命令 解释给 操作系统内核 

### 执行命令的方式  

1.交互式   用户输入一条命令  比如 rm -rf  /* 

2.批处理式的  

​	将多个命令汇集成一个脚本  一次将这次命令执行完毕  

```
#!/bin/bash 
	which ls 
	cd /usr/local/nginx 
	mv test.txt /tmp/
```





### 脚本批处理式  

需求: 如果实线某个需求  同时2个或者三个服务启动   

nginx   

mysql

apache2 

test start   实线下面三个全部启动 重启 停止

service nginx restart|start|stop

service  mysql start|restart|stop

service apahce2 start|restart|stop 

```shell
#!/bin/bash 

if [ $1 == "start" ]
then
service nginx start
service mysql start
service apache2 start
fi

if [ $1 == "stop" ]
then
service nginx stop
service mysql stop
service apache2 stop
fi

if [ $1 == "restart" ]
then
service nginx restart
service mysql restart
service apache2 restart
fi
```



> 开头必须是  #!/bin/bash 
>
> 后缀名 一般 是以 .sh 
>
> 脚本要有执行权限   chmod  +x 脚本.sh 
>
> ./脚本.sh  运行       ./  
>
> /etc/init.d/test.sh   绝对执行  

```shell
#是shell脚本中的注释 第一行比较特殊

#!/bin/bash 

cd /
ls -al 

:wq!


chmod +x 1.sh  

./1.sh 
/tmp/1.sh 
```

## shell 变量  

> 局部变量   只能在当前shell中运行 
>
> 全局变量 应用于整个系统  一般大写  

```
#!/bin/bash 

设置局部变量 
a=10 等号两边不能留空格
b=$a
显示局部变量 
echo $a    10
echo ${b}  10 
echo "a=$a" a=10 双引号解析变量 

销毁变量 
unset a #这时候不要加$


b=`date` 将系统命令结果赋值给一个变量   命令用`` 包起来   
```



位置变量    

```
$0 表示的是脚本的名称 
$1~$9 传递给脚本的参数   
```

## 特殊变量    

```
$# 表示传递给脚本的参数个数
$* 传递给脚本的所有参数
$? 表示命令的返回值  0表示成功   

```

### 常量

```
readonly a=10 
echo $a 

a='66'
echo $a  #报错  表示  a 是常量 不能修改  
```

### 引号

>双引号解析变量 
>
>单引号不解析变量 不解析转移字符 
>
>`` 能够输出命令的结果  
>
>特殊字符 反斜线转义  
>
>& * ？^      
>
>
>
>\  转义字符   

### 字符串   

```
计算字符串的长度  
	#!/bin/bash
	test='123456abc'  ${#字符串的名字}
	echo ${#test}

提取字符串  
	${字符串的名字:开始的位置:截取的长度}

    echo ${test:3:5} 456ab
```

### 数组  

``` 
a=(1 2 3)  #声明一个数组  元素之间 用空格隔开  
获取数组的元素  #{a[0]} 1 
获取数组的长度 echo ${#a[@]} echo ${#a[*]}  
```

## seq  

```
seq 5-10  生成5-10之间的连续整数   

```

## 运算  

####　数学运算　　

```
a=100
echo $[$a+10]
echo $[$a-5]
echo $[$a/2]
echo $[$a*2]
echo $[$a%2]
~             

echo $((5+10))
echo $((10/2))
echo $((10/3)) 3  整除 余数忽略掉了 

```

## 逻辑运算  

&& 

```
if [ $a != 100 && $b ==100 ]

```



|| 

```
if [ $a != 100 || $b ==100 ]
```



## 关系运算符

| 运算符     | 说明          |
| ---------- | ------------- |
| -eq     == | a -eq b  相等 |
| -ne    !=  | a -ne b       |
| -gt    >   | a -gt b       |
| -lt    <   | a -lt b       |
| -ge  >=    |               |
| -le   <=   |               |
|            |               |
|            |               |

## 字符串判断   

| 运算符 | 说明                               |
| ------ | ---------------------------------- |
| =      | 字符串是否相等                     |
| !=     | 不等                               |
| -z     | 字符串的长度是否为0 为0返回true    |
| -n     | 字符串的长度是否为0 不为0返回true  |
| str    | 检测字符串是否为空  不为空返回true |
|        |                                    |
|        |                                    |

## 文件的判断  

| 运算符   | 说明                                       |
| -------- | ------------------------------------------ |
| -d 文件  | 检测是否是目录 是返回true                  |
| -f  文件 | 检测是否是普通文件  （不是目录不是设备 ）  |
| -r 文件  | 检测文件是否只读                           |
| -w 文件  | 检测文件是否可写                           |
| -x 文件  | 是否可执行                                 |
| -s  文件 | 检测文件是否为空 也就是说文件的大小是否为0 |
| -e  文件 | 检测文件是否存在  存在返回true             |

### 分支语句  

#### if-else 

```
#!/bin/bash 

read -p '请输入您的分数:' score
if [ $score -gt 80 ] 中括号前后有空格  
then 
   echo '优秀'
else 
   echo '一般'
fi

if [ $score -gt 80 ];then 
   echo '优秀'
else 
   echo '一般'
fi
```

  



#### case  

```
case $变量名 in    
模式1）
命令1
;;
模式2）
命令2
;;
默认执行的命令
;;
esac  

switch(){
    case 1:
    
    default
}
```



```
#!/bin/bash 

case $1 in
start| begin)
echo 'start'
;;
stop | end)
echo 'stop'
;;
*)
echo 'i dont know'
esac 


./2.sh start  
```



## 循环语句  

```
for 变量 in 列表 
do
   命令1
   命令2
done 

for i in 1 2 3 4 5;do
   echo $i
done

//从命令中读取值   
for j in `cat 1.txt`;do
   echo $j
done

读取目录列表  

for file in /home/*;do
	echo $file
done 

求1-100的和  
for x in `seq 1 100`;do
        let sum+=$x

done
        echo $sum  
        
遍历数组  
a=(1 2 3 4 5 6)
for i in ${a[*]};do
echo $i
done


```

## while  

```
while 条件 
do	
	命令
done


sum=0
i=0
while [ $i -lt 100 ];
do
    let sum+=$i
    let i+=1
done
    echo $sum

```



## 函数  

```
先定义 后使用  

test(){
    echo 'hello world'
}

test
```





切换yum 源头 

```
1.cd /etc/apt  
准备  两个source.list  一个是公司的  一个是阿里云
```



```shell
#!/bin/bash 
rm -rf /etc/apt/source.list
read -p '请选择内网还是阿里云（1 内网 2 阿里云）：' network

if [ $newwork -eq 1 ];then
   echo '你选择了内网'
   cp -r /tmp/source.list /etc/apt/
else
   echo '你选择了阿里云'
    cp -r /tmp/aliyun/source.list /etc/apt/
 fi
 
 sudo apt-get update 
 sudo apt-get upgrade 
```

```
chmod +x 脚本名称.sh 
```



###　远程登录  公钥加密 私钥解密   　　

```
ssh 用户名@ip地址  
exit 退出远程登录   

建立两台主机之间的信任关系 
1.10.8.152.117 
2.10.8.152.95 

 第一步： 在 95上  
 ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): /root/.ssh/id_j_rsa 可以自己起名字防止覆盖掉原来的密钥  英文状态下写  
Enter passphrase (empty for no passphrase):   回车
Enter same passphrase again:  回车  

cat id_j_rsa.pub   
   复制内容  注意 别复制多 也别复制少  
第二步 : 
	在 117 上  
	mkdir -p /root/.ssh  
	cd /root/.ssh 
	vim authorized_keys
	粘贴第一步复制的内容    
	保存   
	
	chmod 600 /root/.ssh/authorized_keys  这一步相当重要   
	
第三步: 在95上  ssh@10.8.152.117 这个时候就不需要输入密码  	
	
```

## 远程复制　

```
scp -r 源地址  目标地址  
scp text.txt root@10.8.152.117:/tmp  将本机的文件 复制到 目标主机的tmp目录下面 
scp root@10.8.152.117:/root/text.txt /tmp
scp -r root@10.8.152.117:/root/www /tmp 将远程
```

### screen   

```
sudo apt-get install screen    

screen -S 自己命名 
执行任务   

ctrl+a+d 退出会话 回到主窗口  

screen -r 窗口名字   进入指定的子窗口    

screen -X -S 窗口名  quit   结束指定的 子窗口 任务   
```





### 磁盘管理  

```
HDD 机械硬盘 
HHD 混合硬盘
SDD 固态硬盘


文件系统类型 
windows ntfs fat32 fat16 exfat 
Linux  ext4 ext3 ext2 
```

### 查看磁盘的使用情况 df  

```
第一块磁盘 sda 
第二块就是 sdb
第三块     sdc

df -h 以最佳阅读体验阅读磁盘的使用情况  
df  -k 以k为单位
df -m 以m为单位

df -h /dev/sda2  
```

## 查看文件 及目录对空间的使用情况 du    

```
du -a 显示所有文件目录大小   包括隐藏文件  
du -h 最佳阅读体验阅读
du -s 只显示目录大小 不显示子目录和文件的大小  
du -c 显示目录及文件的大小  并显示总大小 
```

## 磁盘分区 fdisk  先分区 再格式化 然后挂载    

```
fdisk -l 查看磁盘的分区情况  

发现多了一个 /dev/sdb   

fdisk /dev/sdb   
	m 帮助  
	n 新建一个分区 
	d 删除一个分区
	w 保存 
	q 退出 

新建一个分区  
	新建主分区 primary  然后  extends 扩展分区  
	n -p-默认回车-默认回车-+8G-w 


格式化  
	fdisk -l 发现多了一个 /dev/sdb1  

	mke2fs -t ext4 /dev/sdb1 
	-t表示 制定文件系统类型   


挂载   
	mkdir -p /python1804
	mount -t ext4 /dev/sdb1 /python1804   临时挂载  注销 重启 这个磁盘就没了 
	df -h
	发现 多了一个/dev/sdb1 
取消挂载 
	umount /python1804/
	df -h 
	/dev/sdb1 就没了  

永久挂载 
	vim /etc/fstab 
	/dev/sdb1       /python1804     ext4    defaults        0 0
	新磁盘			 挂载点 		文件系统类型  默认参数 	 0 是否需要备份 0不备份   0 是否开机检查磁盘 0 不检查  
	
	mount -a 让挂载立即生效  
```



### 别名   

```
有的命令 加参数 很繁琐 可以根据团队的要求  重新命名  为了提高效率  

vim /root/.bashrc 
alias 新名字='命令 参数'
alias c='cd'

source /root/.bashrc #让配置未见立即生效  


```

