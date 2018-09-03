Linux 第二天

## 复习 

```
ls --help 
cd 
ls 
ifconfig 查看网卡信息 及IP地址    ipconfig 是windows 下面查看的方式  
sudo  reboot|init 6
sudo  halt|poweroff|shutdown -h now|init 0 
whoami 查看当前登录的用户是谁  
date 查看日期和时间
cal 查看日历 
cal 2018 查看一整年的日历 
sudo apt-get install 
			remove
sudo apt-get update
			upgrade
上箭头   上一条命令 
下箭头   下一条命令 
```

## 认识Linux的 架构  

```shell
linux中  一切都是从 跟出发   / 
在Linux中一切皆文件  设备也是文件  访问设备的方式跟访问文件的方式 是一样的  
挂载 ： 好比把U盘放到 电脑上  本来U盘是不能直接访问 的  电脑可以访问 硬盘的内容  我们通过把U盘 插入到 电脑上 实线访问  这个过程就叫做挂载  

.   #跟   
├── bin # 存放我们常用的命令 比如cd ls 
├── boot #我们系统启动文件 和 核心文件都在这里存着   不要动
├── cdrom # 光驱 的内容
├── dev # device 存放我们的硬件设备 硬盘 光驱在这里  						重点 
├── etc #系统的配置文件 放在这个目录下面     								重点
├── home #普通用户的家目录 												重点   
├── lib #系统的动态链接库   类似于windows 中的.dll文件
├── lib64 #64位操作系统的动态链接库  
├── lost+found #系统自动生成的 如果文件系统出错  这里边会自动多文件 记录产生的错误 
├── media #当我们插入upan 或者光驱的时候 会自动挂载到这里   
├── mnt # mount 挂载的意思 Linux 文件系统类型是ext4 windows 中 是 NTFS ext4想访问ntfs类型 可以手动挂载到mnt 下面   重点  
├── opt #安装第三方软件 比如Oracle 等 
├── proc # 系统的进程 状态信息 存放在这里 这些信息是在内存中  
├── root #管理员用户的家目录 									重点  里边好多隐藏文件  千万别动 
├── run  # 进程的运行数据
├── sbin # 存放管理员常用的命令  			
├── snap # sudo snap  
├── srv  # Linux服务信息
├── sys	 #在这里存放Linux驱动的实时信息    
├── tmp  # 临时目录															次重点
├── usr  # 	C:\Program Files 类似于windows 的这个目录安装软件安装在这里			重点				bin 应用程序的可执行文件
	sbin 超级管理员的执行文件
	local 管理员安装应用程序的目录
├── var
	# 用来存放不断变大的文件  比如数据库文件    日志   进程          重点  
└── vmlinuz -> boot/vmlinuz-4.13.0-36-generic

```

## 历史命令  

```shell
history 

!行号 #自动执行第n条历史命令 
```

## 目录的相关操作   

```
ls  
	-a 列出所有的子目录及文件  包括隐藏的
	-l 详细信息的形式展示
	--color 不同的颜色展示  
		文件 白色
		文件夹 蓝色
		链接 浅绿色
	
```



## 文件及目录的属性 

```shell
-rw-r--r--   1 root root     0 7月  18 10:11 text.txt
drwxrwxrwt  10 root root  4096 7月  18 10:01 tmp
drwxr-xr-x  11 root root  4096 3月   1 02:35 usr
drwxr-xr-x  14 root root  4096 3月   1 02:52 var
lrwxrwxrwx   1 root root    30 5月   7 19:36 vmlinuz -> boot/vmlinuz-4.13.0-36-generic

第一部分:
	-  文件  
	d  目录 文件夹
	l  链接  
第二部分:
	rwx  文件或者目录拥有者的权限
	r--  所属组的权限 
	r-x  其他人的权限 
	
	r  read
	w  write
	x  exec
	-  表示没有权限
	

第三部分
	10 inode节点  
第四部分
	root  所属的用户  
第五部分
	root  所属的组  
第六部分
	4096 文件的大小  以字节为单位 
第7部分
	5月   7 19:36 最后一次的修改时间   
第八部分
	目录或者文件的名字


```

## 软连接   ln 就是windows中的快捷方式   

```
link 
ln -s 源文件的名字 新名字(快捷方式的名字)

sudo ln -s text.txt tt 
lrwxrwxrwx   1 root root     8 7月  18 10:52 tt -> text.txt
```

## 目录管理   

pwd 查看当前所在的目录  会显示完整的路径

**绝对路径** 

​	cd /home/jinxingping/    一切从/出发 

 **相对路径** 

​	以当前目录为准  上级目录 或者 字目录 

​	..  回到上级目录

​	../../ 回到上两级目录

​	.  还是在当前目录  

```shell
cd    #回到当前用户的家目录
cd .. #回到上级目录
cd .  #还是在当前目录 
cd ~  #回到用的家目录
cd -  #回到来源的目录
cd /  #回到根目录 

```

### 创建目录

```shell
sudo mkdir 目录名字  # 创建目录
sudo mkdir -p 目录名/子目录名/孙目录名 

sudo mkdir -p xingyun/{xiaoxingyun,xiaoyunyun}/{feifei,xiaofeifei,feifeifei} #同时创建多个递归目录
└── xingyun
    ├── xiaoxingyun
    │   ├── feifei
    │   ├── feifeifei
    │   └── xiaofeifei
    └── xiaoyunyun
        ├── feifei
        ├── feifeifei
        └── xiaofeifei
```



 ## 删除 目录

```
sudo rmdir 目录名 
sudo rmdir -p #递归删除  目录里边不能再有子目录或者文件  
```



## 文件的操作  

### 文件的创建   

```
sudo touch 文件名 
sudo touch	文件1 文件2 ..... 文件n  #同事创建多个文件
cat 文件名  查看文件中的内容
echo 内容 #输出内容到屏幕上 
echo 内容 > 文件名  将内容输入到文件中  
echo 内容1 > 文件名1  原内容会被替换掉 
echo 种花没意思,我们一起中草莓 >> 2.txt 追加到文件的结尾  

```

## 文件的移动 

```
mv  全拼 move 

sudo mv 2.txt /home/   将2.txt 移动到 home 目录下  
sudo mv 1.avi xingping.avi 重命令 
sudo mv 目录名  已经存在的目录名/ 将目录放到已经存在的目录下面  
sudo mv 文件名或者目录名   新目录名  重命名 
```



## 文件的复制  

```
cp  copy  
sudo cp sources.list sources.list.backup 相当于备份  
sudo cp -r 目录名  新目录名  复制目录  
```

linux 不区分文件后缀名  .txt .avi 一样一样的 

## 文件的删除   

```shell
sudo  rm  文件名   删除文件  
sudo rm 1.txt 2.txt  同时删除多个文件  
sudo rm -i 删除之前确认  
sudo rm -f 强制删除 
sudo rm -f *.mp4 删除.mp4结尾的文件 
```



## 强制删除目录 及文件 

```
sudo rm -rf  目录名/文件名   
慎用  rm -rf /* 
```



## 查看文件  

```
cat 文件  #查看文件的内容 
tac 文件  #cat 的反写 从后往前输出  
jinxingping@jinxingping-Parallels-Virtual-Platform:/tmp$ tac 4.rmvb 
你长得好像一个人
他强任他强,老子丁彦雨航
jinxingping@jinxingping-Parallels-Virtual-Platform:/tmp$ cat 4.rmvb 
他强任他强,老子丁彦雨航
你长得好像一个人
jinxingping@jinxingping-Parallels-Virt

head -n 数目  文件名  
head -n 10 /etc/passwd # 只查看 /etc/passwd 的 前10行 
tail -n 2 /etc/passwd # 后2行

tail -f cat 文件名 #实施查看文件后面的内容  用来查看日志 用
watch -d -n 秒数 cat 文件名 #每秒刷新一次文件的内容       重点   


more 文件名  
	空格  分页查看 
    回车  一行行的查看   
    q 退出 
less 文件名  
	g 首页  
	G  尾页 
    b向前分页  
    空格向后翻页  
    q 推出  

stat 文件  查看文件的详细信息    
	
	
	atime accesstime 访问时间
	mtime modifytime 修改时间 
	ctime createtime 创建时间 

```

### 文件的查找  

```
find  [路径] [参数] 【文件的名字 】
			-name 按照文件名字查找
			-iname 文件名忽略大小写
			-mtime	+n 表示超过n天的    -n 表示n天以内的文件  
			-user  根据文件所属的用户 查找
			-size  单位  c 字节 k m g 
find / -name test.html  在/目录下面 查找文件名为 test.html 的文件   
find /tmp -mtime -1 查找tmp 目录下  修改时间在一天以内的  
find /tmp -user root 查找tmp 下面 属于root 的文件  
find /tmp -size 10c 查找tmp 下面大小为10个字节的文件 
find /var -szie +10k -size -100k -name "*.log"
#查看var 下面文件大小问  大于10k 小于100k 的 *.log 的文件 

```

## grep 类似于js 中的正则表达式   

```
grep 参数  '内容' 文件名   
$ grep jinxingping /etc/passwd 
jinxingping:x:1000:1000:jinxingping,,,:/home/jinxingping:/bin/bash

	-i 忽略大小写 
	-c 只显示行数 不显示内容  
		jinxingping@jinxingping-Parallels-Virtual-Platform:/tmp$ grep -c jinxingping /etc/passwd    返回1  
	-r 递归查找  
	-n 显示行号   
		grep -n jinxingping /etc/passwd  显示行号+内容 
	-w 只匹配单词   
	--color 不同的颜色显示   
	grep -c jinxingping --color /etc/passwd 
	grep -c jinxingping --include "*.py" #在*.py的文件中搜索jinxingping 
	grep -c jinxingping --exclude  "*.py" # 不在*.py的文件中搜索jinxingping 
```



| 管道符

```
上一个的输出 作为 我下一个的输入    
cat /etc/passwd | grep 'root'
先输入 /etc/passwd 中的内容 然后 然后匹配 包含root 的行  

```

## 查找命令所在的位置  

```
/bin 存放我们普通用户常用的命令 

which 命令   
which ls 
	/bin/ls 
whereis ls 
	
```

## wc 统计  

```
 cat /etc/passwd | wc -l  #统计/etc/passwd  有多少行  
 
 wc -l 行数
 	-w 统计单词数   
```



## awk  流式编辑器 将每一行 进行切割 然后 进行分析     

```
cat /etc/passwd | awk -F ':' '{print $1}'
-F 以: 进行切割    
$1 打印第一部分
$0 打印全部
```

## uniq 配合sort 来使用  

```
uniq -c 文件名  输出每一行出现的次数   

banana
apple
banana
apple
orange
pear


jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sort test.txt | uniq -c 
      2 apple
      2 banana
      1 orange
      1 pear
jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sort test.txt | uniq -d
apple
banana

```

## sort 排序  

```shell
cat /etc/passwd | awk -F ':' '{print $3}' | sort  -n  -r

sort 
	-n 按照纯数值排序  
	-r 倒序 
	-k 按照指定的列排序 
	-t 指定分隔符 
	-u 忽略相同的行  
cat /etc/passwd | sort  对第一个字母进行排序
cat /etc/passwd | sort -t ":" -k 3 -n   以：为分割点  然后对第3列进行排序  按照纯数值的方式牌组 
```



## 查找你最近经常使用的10条命令  

> 最经常使用 history  
>
> uniq -c 显示出现的次数     
>
> sort 排序  
>
> 然后对第一列 进行 排序 按照纯数字 倒叙排列   
>
> 取前 10行  

```
history | awk -F " " '{print $2}'|sort |uniq -c | sort -r -n -k 1 | head -n 10 


```

 



## 文件的权限 操作  

```
-rw-r--r--   1 root root     0 7月  18 10:11 text.txt
drwxrwxrwt  12 root root  4096 7月  18 15:37 tmp
lrwxrwxrwx   1 root root     8 7月  18 10:52 tt -> text.txt
drwxr-xr-x  11 root root  4096 3月   1 02:35 usr
drwxr-xr-x  14 root root  4096 3月   1 02:52 var

r 读   100		 4 
w 写	  010 		 2
x exec 执行  001   1
- 没有权限 

rwx  文件或者目录的拥有者
r-x  所属组的权限 
r-x  其他用户的 

755    
rwx 
r-x
r-x 

777 
rwx 
rwx 
rwx 

600 
rw-
---
--- 

700 
rwx 
---
---

644 
rw-
r--
r--


666 
拥有着  读写 
组  读写 
其他用户 读写 


u 代表拥有者
g 所属组
o 其它人


```

### chmod  

```
chmod 	
	 + 增加权限  在原来的基础上  
	 - 减掉权限 			  
	 = 赋值权限    
chmod +x test.txt   三种身份全加 
	  -w text.txt   全部减  
	  g+x test.txt  只给所属的组加上执行的权限 
chmod 777 test.txt  给test.txt所有人最大的权限   
	  666
	  644
	  755 
	  
chmod -R 777 目录  递归修改目录及子目录及文件  
```



> 哪个目录或者文件  先到它所在的上一级目录 然后再 修改  

## 更改文件 及目录的拥有者 chown   

```
change owner 

chown 用户名 文件或者目录  
chown 用户名:组名  
chown :组名 
chown -R 用户名:组名  
```



## 更改所属的组 chgrp   

```
chgrp 组名 文件名/目录名 
chgrp -R  组名  文件名/目录名   递归修改
```



## chattr 底层控制权限 chmod 背后的大boss          

```
i 不能删除 不能重命名 不能修改权限 
a 只能追加 不能删除    只能 >> 
+ 在原来的基础上增加
- 在原来的基础上递减 
= 更新权限  

jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sudo chattr +a text.txt 
jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sudo echo '我很怕你,为什么怕我,因为我怕老婆' >> text.txt 
jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sudo echo '我很怕你,为什么怕我,因为我怕老婆' >> text.txt 
jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sudo echo '我很怕你,为什么怕我,因为我怕老婆' >> text.txt 
jinxingping@jinxingping-Parallels-Virtual-Platform:/$ sudo rm -rf text.txt 


 sudo cat /etc/passwd >> text.txt
```



## 用户和组的管理  

> 一个用户 至少有一个主组  
>
> 一个组可以拥有多个用户  
>
> 一个用户可以拥有多个组

###  添加用户 useradd   whoami  查看当前是哪个用户登录       

```
useradd tengfei  
su 用户名  #切换用户 
su 不写用户名   表示 切换到root 
添加用户之后  用户信息 会存到  /etc/passwd   重启以后 还会在 home下面产生一个 以用户名为命名的目录  表示该用户的家目录  

jinxingping:x:1000:1000:jinxingping,,,:/home/jinxingping:/bin/bash

第一部分:jinxingping  用户名
第二部分:x  用户的密码 
第三部分:1000 用户的id   
第四部分：1000 组id  
第五部分:/home/jinxingping 家目录  
第六部分: /bin/bash  表示jinxingping 具备执行脚本的权限    
		/usr/sbin/nologin  表示该用户不具备执行脚本的权限   也表示 该不能用该用户登录系统

useradd  
		-g 添加用户的时候 直接把该用户加入到指定的组中 组必须是已经存在的 
		sudo useradd -g jinxingping jinlong 
		-u 用户制定id 
		sudo useradd -u 8888 liangliang
		-m 在创建 用户的同时 自动在home 目录下面生成一个以用户名为命名的目录
		-d 制定用户的家目录  
		sudo useradd -md /qiaoqiao  jinqiao  创建jinqiao的同时自动创建用户家目录  并且自己制定家目录在 /qiaoqiao  
		
		-s 制定用户不能登录  
		sudo useradd  -m guoguo -s /usr/sbin/nologin 
```

### 删除用户 

```
userdel 用户名  
```







### 修改用户密码 passwd   不是   password  

```
passwd 用户名  
passwd  后面不写用户名 默认修改root 
	Ubuntu root 是不启用的 

	启用管理员用户   sudo password root  
	su root  
passwd -l 锁定用户密码
	   -u 解锁用户密码  
```



### 用户组  

```
groups  查看当前用户属于哪个组  
id 查看用户的id  组id 

添加组 
	sudo groupadd 组名  
	
	组的信息存放在  /etc/group 下面   
	
	kangbazi:x:8892:
	组名:组密码:组id 
	
删除组 
	sudo groupdel 组名 

修改组名 
	sudo groupmod -n 新组名 旧组名 


```



















