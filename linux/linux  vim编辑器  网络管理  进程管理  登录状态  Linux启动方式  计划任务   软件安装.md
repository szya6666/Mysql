# linux  vim编辑器  网络管理  进程管理  登录状态  Linux启动方式  计划任务   软件安装     



## vi  vim编辑器    

```
sudo apt-get install vim  
```

### 编辑模式  vi 文件 先进入命令模式   然后 i o a s 进入编辑模式    英文状态下  

| 按键 | 作用                          |
| ---- | ----------------------------- |
| i    | 在光标所在的位置插入元素      |
| o    | 在光标的下一行输入            |
| esc  | 回到命令模式                  |
| a    | 在光标的下一个位置输入内容    |
| s    | 先删除光标所在位置的字符      |
| S    | 删除光标所在行的内容 然后输入 |
| I    | 在光标所在行的行首输入        |
| A    | 光标所在行的行尾输入内容      |
|      |                               |

> 重点  i o a esc     

### 命令模式   英文状态  

| 按键    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| h       | 向左移动一个字符                                             |
| j       | 向下移动一个字符                                             |
| k       | 向上移动一个字符                                             |
| l       | 向右移动一个字符                                             |
| yy      | 复制                                                         |
| p       | 粘贴                                                         |
| np      | 粘贴n行                                                      |
| nyy     | 复制n行 如果不足n行  实际有多少就复制多少 超过n行  就复制n行 |
| gg   (  | 回到第一行行首                                               |
| dd      | 删除1行                                                      |
| ndd     | 删除n行                                                      |
| u       | 撤销上一次的操作 window中ctrl+z 一样                         |
| GG    ) | 回到最后一行                                                 |
| .       | 重复上一次的操作                                             |
|         |                                                              |
|         |                                                              |
|         |                                                              |
|         |                                                              |
|         |                                                              |

> 编辑模式完成以后 先回到 命令模式 然后 在进入底部命令模式 

## 底部命令模式   :     英文状态下输入  

| 命令                                | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| :w                                  | 保存 不退出                                                  |
| :q                                  | 不保存 退出  quit                                            |
| :wq                                 | 保存并退出                                                   |
| :wq!                                | 强制保存并退出                                               |
| :x 相当于 wq                        | 保存并退出                                                   |
| :set nu                             | 设置行号                                                     |
| :行号                               | 定位到所在的行                                               |
| /内容   然后回车                    | 查找文章中的目标内容  n 下一个    从上往下                   |
| ?内容  回车                         | 查找文章中的目标内容  n 下一个    从下往上查找               |
| :s/要查找的字符串/要替换的字符串    | 将制定的内容替换成新的内容 但是 只是替换光标所在的行  多个制定的内容只是替换一个 |
| :s/要查找的字符串/要替换的字符串/g  | 将制定的内容替换成新的内容 但是 只是替换光标所在的行  多个制定的内容全部替换 |
| :%s/要查找的字符串/要替换的字符串   | 将制定的内容替换成新的内容 替换所有的行   多行制定的内容只是替换第一个 |
| :%s/要查找的字符串/要替换的字符串/g | 将制定的内容替换成新的内容 替换所有的行   多行制定的内容全部替换 |
| %  g                                | : /  当作字符串显示  记得转义                                |
| 特殊符号记得转义                    | :%s/http\:\/\/www.qfedu.com\/1.html/https\:\/\/www.so.com\/1.py |

## 网络管理  

```
ifconfig  查看网卡的信息   

enp0s5    Link encap:以太网  硬件地址 00:1c:42:d1:b3:db  
          inet 地址:10.8.152.68  广播:10.8.152.255  掩码:255.255.255.0
          inet6 地址: fe80::c675:485f:c17e:7b02/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  跃点数:1
          接收数据包:242823 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:7769 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:62222196 (62.2 MB)  发送字节:799526 (799.5 KB)

lo        Link encap:本地环回  
          inet 地址:127.0.0.1  掩码:255.0.0.0
          inet6 地址: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  跃点数:1
          接收数据包:358 错误:0 丢弃:0 过载:0 帧数:0
          发送数据包:358 错误:0 丢弃:0 过载:0 载波:0
          碰撞:0 发送队列长度:1000 
          接收字节:29137 (29.1 KB)  发送字节:29137 (29.1 KB)
          
enp0s5  这就是我们虚拟机的网卡名称      
inet	ipv4 IP地址   
	    网关 
	    子网掩码    
	    一个电脑 ip地址  三者缺一不可  
inet6    ipv6的地址 
lo		每个网卡 都有一个回环网卡  ip地址就是 127.0.0.1  

接收数据包:358 错误:0  表示网络正常通信  
接收数据包:0 错误: 358 网络连接失败  



ifconfig enp0s5  查看制定网卡的信息  

ifconfig enp0s5 up 启动网卡 
ifconfig enp0s5 down 关闭网卡 

重启网络服务  

service  networking restart|start|stop 
/etc/init.d/networking restart|start|stop
```

## ping   

```
ping  -c 20 www.baidu.com  指定ping20次  
	  -b www.baidu.com	 测试网关 到百度的连通情况 
```

## netstat  查看网络连接状况  

```
-a 显示所有
-t tcp 协议  记录通过tcp 协议连接过来的
-u udp 协议  记录通过udp 协议链接过来的  
-n 显示端口号   
-p 显示进程  
协程  进程  线程 


root@jinxingping-Parallels-Virtual-Platform:/home# sudo netstat -nt   显示所有的tcp链接
激活Internet连接 (w/o 服务器)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0    308 10.8.152.68:22          10.8.152.36:51198       ESTABLISHED

sudo netstat -nt | grep 22 
 
 
sudo netstat -ntpa tcp链接过来的端口号 占用情况   





```

  



## 进程管理  类似于Windows的任务管理器  CTRL+shift+esc中的进程  

```
ps   process status 的简称    查看当前系统进程状态  
	 -a 显示所有的进程  
	 -u 制定用户的进程  
	 -x	跟a 配合使用 显示详细的信息 
	 
	  ps -u root | more -10  显示root用户的进程  每一页显示10条  
	  ps -aux | grep ssh  #显示正在内存中的程序 匹配ssh   
	  
	  
	  USER       PID       %CPU                %MEM
	  用户	   进程号	  该进程占用了多少cpu	该cpu占用了多少内存
	  
	  tty 近程的控制终端  ？ 表示 不是通过终端进来的  pts 远程过来的   
	  start 进程开始时间   
	  
	  先用 ps -aux | grep    查看对应的 pid 
杀死进程  
	  kill -9 pid 进程号
所有相关的进程 全部干掉 
	  killall -TERM sshd 不用关心具体的进程号是多少   进程相关的全部干掉   
```



 ## top  windows任务管理器中的性能  

```
top   

top - 11:41:14 up  3:03,  2 users,  load average: 0.06, 0.05, 0.01
Tasks: 171 total,   1 running, 170 sleeping,   0 stopped,   0 zombie
%Cpu(s):  1.7 us,  1.0 sy,  0.0 ni, 97.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1003736 total,   146148 free,   614244 used,   243344 buff/cache
KiB Swap:  2095100 total,  2074876 free,    20224 used.   212624 avail Mem


11:41:14 当前的时间 
up  3:03 系统运行了 3个小时3分钟  
2 users 当前登录用户数量  

load average: 0.06, 0.05, 0.01
系统的负载   每一分钟  每五分钟  每十五分钟  
单核 这个值不能超过1   压力越大 这个值越接近于1  
双核 不能超过2     

171 total  总共171个进程  
 1 running  1个正在运行 
 170 sleeping 睡眠   
 stopped 没有停止的进程  
 zombie 僵尸进程  
 
 
 KiB Mem :  1003736 total,   146148 free,   614244 used,   243344 buff/cache
KiB Swap:  2095100 total,  2074876 free,    20224 used.   212624 avail Mem

 物理内存   1003736KiB   一个G   free 空闲    used 被利用   buff/cache  内存缓存的数量  
 
 KiB Swap:  2095100 total,  2074876 free,    20224 used.   212624 avail Mem   
 交换分区  将硬盘中快的部分 分出来  当内存使用    
 
 
  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+            COMMAND 
  进程  用户     优先级 	传送广播包的一个带宽      	消耗自愿书			cpu利用率 内存利用率  运行时间 
 
 
 主要查看   cpu  内存  利用率 以及负载情况  
```

## htop  

```
sudo apt-get install htop   

htop  
	可以直接手动选择 然后f9 kill掉
```



## 登录状态 管理  uname   

```
uname -r 内核版本号  
uname -v 系统的版本号  
uname -a 显示系统的所有信息  


hostname  获取主机名 
sudo hostname 新名字  设置主机名  


永久的设置主机名: 
	sudo vim /etc/hostname   
	修改之前 最好记得备份


whoami 查看当前登录的用户  


who 查看终端 及远程 用户谁登录  通过什么方式 登录   
w 查看登录用户的行为 及负载  

 14:20:14 up  5:42,  2 users,  load average: 0.01, 0.05, 0.01
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
jinxingp tty7     :0               08:38    5:42m  1:09   0.11s /sbin/upstart --user
jinxingp pts/18   10.8.152.36      14:10    3.00s  0.11s  0.00s w

last   查看最近用户登录的信息  
last -10 显示最近10个登录的用户的信息   

	
```



## Linux 的启动级别  

GUI 图形界面  

> 电源-> 加电自检-> 读取mbr引导分区 -> 加载Linux内核 ->加载Linux init进程->系统初始化 ->看到画面   

```
vim /etc/rc-sysinit.conf   
设置启动级别   
第14行  env DEFAULT_RUNLEVEL=2

Ubuntu 默认启动级别 2     redhat 默认启动级别  3  					
Ubuntu  															redhat
0   关机															 关机
1   单用户模式  以root的身份开启一个虚拟控制台 主要用来 管理系统用  		单用户模式
2   带显示管理的GUI 完整多用户模式                    					多用户模式 但是没网络
3	带显示管理的GUI 完整多用户模式    								完整多用户模式
4   带显示管理的GUI 完整多用户模式 									留给用户自定义
5   带显示管理的GUI 完整多用户模式 									图形界面模式 从命令行切换到																  图形界面
6  重启
```



> 2345   



## linux 定时任务  人工不干预 完全让服务器执行操作     

```
js中定时任务   
	超时 间歇调用    
第一种方式: 修改配置文件  
vim  /etc/crontab     

分 时 日 月 周  用户  命令    
45 14 19 7 4   root echo '' >> /text.txt    7月19号周四 14点45分  执行 将内容追加到根目录下面的text.txt 中   


第二种 : crontab 命令 


crontab -l 列出所有的定时任务    看不到配置文件中写的定时任务  
crontab -e 新增计划任务  跟上面的区别在于  没有用户名 
crontab  -r 清空计划任务  
分  时 日  月  周   命令    
55 14  19  7  5    echo '' > /tmp/text.txt   
*  *   *   *  *    命令   每分每时每天每月每周 执行这个命令  
0-59 0-23 1-31 1-12 0-6  0是 周天 1-6 周一到周六      
0  2   *   *   *   mysqldump  每天的2点备份数据库  
0  2   *   *   2   sync    每个周二的2点做数据同步     
0  8   15  *   *   /home/jsgz.py 每个月15号的八点给大家算工资  
0  */2 *   *   *   /home/camera.py 每隔2个小时执行一次查看摄像头  
0  8,12,18 * * 1-5 kq.py    每周1-5 的 8点 12点 18点  执行打卡  
0  8-22  * * *  study.py    每天 8点到晚上10点 在教室学习


定时服务  

service  cron start|stop|restart   

查看服务是否启动  
ps -aux | grep cron  
root     19134  0.0  0.2  36076  2924 ?        Ss   15:07   0:00 /usr/sbin/cron -f   进程
root     19246  0.0  0.1  21312  1016 pts/18   S+   15:07   0:00 grep --color=auto cron 守护进程 

```



## 软件安装   

	### 压缩 与 解压缩   

```
windows 常见的压缩包   rar zip 7zip iso  
Linux 常见压缩波    zip gz bz2 tar   
```

## gz 的压缩 和 解压缩    

```
gzip 文件名 
gzip 文件1 文件2 文件3 支持批量压缩   
ps:源文件 不存在 生成.gz的文件    
gzip -d 1.txt.gz 2.txt.gz 3.txt.gz 4.avi.gz 5.yuting.gz  解压缩  支持批量解压缩   源文件被删除   

不能压缩目录   源文件 被删除  
```

## bz2的压缩与解压缩   

```
root@testping:~# bzip2 1.txt 2.txt 3.txt 4.avi 5.yuting 
root@testping:~# ls
1.txt.bz2  2.txt.bz2  3.txt.bz2  4.avi.bz2  5.yuting.bz2  源文件被删除  生成.bz2的文件 

bzip2 -d 1.txt.bz2 2.txt.bz2 3.txt.bz2 4.avi.bz2 5.yuting.bz2   原文件被删除  生成的是去掉bz2的文件   

不能压缩 目录   
```



### tar 打包的意思   跟搬家打包一个道理  

```
tar 
	-c  打包  
	-v  可视化  显示打包解包的过程 
	-f 制定的文件名  
	-t 查看包里的内容 
	-x  解包   
    
    
    -z 以gzip 压缩  解压缩 
    -j 以bzip2 压缩解压缩
	
	tar -cvf kangbazi.tar 1.txt 2.txt 3.txt 4.avi 5.yuting test test1/ 打包后源文件还存在   
	tar -xvf kangbazi.tar  解包 源文件也存在 
    
    gzip kangbazi.tar
    gzip -d kangbazi.tar 
    
    bzip2 kangbazi.tar
    bzip2 -d kangbazi.tar
	
	打包 和解包  只是放进去  拿出来   并没有压缩   
	
	
	gz  打包 并压缩   
	tar -zcvf kangbazi1804.tar.gz 1.txt 2.txt 3.txt 4.avi 5.yuting test test 
	源文件没有被删除 目录也可以被压缩     
	
	gz 解包 并 加压缩   
	tar -zxvf kangbazi1804.tar.gz    源文件还存在 
    
	bz2 打包并压缩  
	tar -jcvf kangbazizz1804.tar.bz2 1.txt 2.txt 3.txt 4.avi 5.yuting test   
	
	bz2 解包  并  解压缩   
	
	tar -jxvf kangbazizz1804.tar.bz2
	
	
	xz  打包并压缩    
	tar -Jcvf kangbazi.tar.xz 1.txt 2.txt 3.txt 4.avi 5.yuting test
	
	xz 解包 并 解压缩  
	tar -Jxvf kangbazi.tar.xz
```



  ```
以下载  apache 为例子  

http://httpd.apache.org/download.cgi  

复制链接  http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.34.tar.gz

切换到 你指定的目录  
wget -c http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.34.tar.gz  
curl -O http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.34.tar.gz  
下载下来   


解包并解压缩
tar -zxvf httpd-2.4.34.tar.gz
  ```



##  zip unzip 

```
sudo apt-get install zip unzip 

zip 
	zip kangbazipython1804.zip 1.txt 2.txt 3.txt 4.avi 5.yuting test/
	源文件 还存在  也可以 压缩目录 
unzip 
	unzip kangbazipython1804.zip
```

### 软件安装    

redhat 阵营  debian 

Ubuntu 所有的安装包都是  .deb   

redhat 所有的安装 都是 rpm 结尾的   rpm（redhat package management）

* apt-get   

  * 解决了dpkg 顺序关系  依赖关系    
  * 安装 搜狗 需要什么软件 自动下载下来然后自动安装   

  ```
  sudo apt-catch showsrc 包名  #查看软件包的信息  
  sudo apt-get resource 包名 #获取软件包源码 
  sudo apt-get install 包名 #安装 
  sudo apt-get remove 包名 #卸载  
  sudo apt-get update 获取新的软件包列表  从新的yum源上 
  sudo apt-get upgrade 有哪些软件包可以更新 检查后更新   
  ```

  

* dpkg 安装   debian package  就类似于 我们安装 QQ先下载  .exe 然后下一步  

  ```
  dpkg -l | grep zip  # 查看 已经安装的 包含zip的软件包  
  dpkg -i sogoupinyin_2.2.0.0108_amd64.deb  安装软件  
  dpkg -r  .deb 报名  卸载软件  不删除配置信息 
  dpkg -P  包名  卸载软件的同时  将配置信息 一起删除  
  ```

  优点: 下载下deb的包 安装就好  

  缺点: 有依赖关系  安装4 前提先安装1 然后2 再3 再4 顺序错了 不行 

* 编译安装  

  * 麻烦 但是 机器喜欢   将软件代码 转化成机器识别的代码  这个过程就叫做编译   



## apt-get 安装 apache 

> 用户在浏览器输入域名  先到 C:\Windows\System32\drivers\etc 检查 hosts 文件中是不是有 该域名  如果有查看对应的ip地址  然后浏览器 就会连接到该ip地址 
>
> 如果没有  会通过dns服务  加速连接到 顶级域名服务器 将 域名 解析成ip 地址 并返回给 浏览器 然后 浏览器就访问该ip地址     

```
sudo apt-get update 
sudo apt-get upgrade   
--------------------- 以上两步 可以不用执行  网速好的话 可以执行一下   
sudo apt-get install apache2

service apache2 start|stop|restart

ps -aux | grep apache2  启动服务 


vim /var/www/html/index.html   

修改成你想要的内容    


在浏览器输入 ip地址  即可    

http://10.8.152.68/index.html 访问   

现在想  www.kangbazi.com/index.html 访问    


打开windows C:\Windows\System32\drivers\etc  

将 hosts文件 拷贝到桌面或者d盘  

在里边添加一行  
10.8.152.68 www.kangbazi.com 
保存  然后 把他 将原来C:\Windows\System32\drivers\etc 下面的hosts 替换掉     这个hosts 只对自己的电脑生效 不是对所有人生效  

接下来 就可以在浏览器   
www.kangbazi.com/index.htm 访问了  


域名申请: 
域名备案:
域名解析: 将域名绑定到你的服务器的ip地址  
购买服务器  123.23.45.56 

xiaoguo.com    
www.xiaoguo.com 
blog.xiaoguo.com
text.xiaoguo.com 


在哪买的服务器在哪里备案  在阿里云买的就在阿里云   阿里云提供 解析   
https://www.dnspod.cn/ 也可以解析 将你的域名解析到 你的服务器上  

每个人都要有自己的  域名和服务器   





域名弄好了 www.zcbhqq.cn 
服务器 弄好了182.61.49.222  
https://www.dnspod.cn/ 添加a记录 解析好了    
ping www.zcbhqq.cn   如果出现 182.61.49.222  说明解析成功   

远程链接上 182.61.49.222  

cd /etc/apache2/sites-available  
sudo cp 000-default.conf www.zcbhqq.cn.conf


vim www.zcbhqq.cn.conf  

<VirtualHost *:80>
      
        ServerName www.zcbhqq.cn   绑定的站点  
        DocumentRoot /var/www/html/zcbhqq.cn 访问过来的地址 要看的内容 在这个目录下 
        ErrorLog ${APACHE_LOG_DIR}/error.log #错误日志
        CustomLog ${APACHE_LOG_DIR}/access.log combined #访问日志
</VirtualHost>

service apache2 restart  重启apache 服务   

















```



  











