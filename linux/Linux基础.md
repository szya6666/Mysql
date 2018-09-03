# Linux基础   

## 操作系统 

​	内核+软件  

​	内核:操作系统的核心 控制着 软件使用硬件资源  

​	软件: GUI图形界面软件   命令行软件 

​	Linux下面  软件都是 c c++写的   桌面也是软件的一种    

​        分类:32位和64位    

​		内存寻址空间 2的32次方 -1   最大支持4gb内存

​					2的64次方-1   支持更大的内存   



Linux有两大阵营: 

 * redhat   redhat\centos
 * debian   debian ubuntu deepin  

> redhat 商用Linux  
>
> centos 社区版Linux 免费  
>
> Ubuntu 有好的图形界面 
>
> fedora  个人版的redhat  
>
> deepin 国产Linux发行版     
>
> 麒麟  国产的    



Linux 特点: 

 * 速度快 消耗资源少 
 * 免费 开源  
 * 稳定 安全 长时间不用关机  
 * 多用户 多任务   
 * 支持多种平台   



安装 Linux  

* 虚拟化  cpu 首先要支持 虚拟化      
* 现在80的服务器 都是安装了虚拟机     
* vmvare 、virtual box （Oracle）



## 连接Linux  

* 终端连接  

* 远程连接    

  * Linux 要开启ssh 服务  

  ```
  sudo apt-get install openssh-server 先安装上openssh-server    
  sudo service ssh start 开启ssh 服务   也就是 开放22端口  
  ```

  * window 和 Linux之间文件传递  

  ```
  scp 协议  走22端口   软件 ：winscp 
  ```

  

## 常用命令   

```
cd 切换目录  目录就是文件夹      
ls 查看目录下面有所有的文件   
sudo 默认 Ubuntu root 权限 不开启 kangbazi  sudo 相当于加了一层保护  会询问你密码   
apt-get install 软件名  安装软件  
sudo poweroff 
sudo init 0
sudo shutdown -h now|+10|18:00 
sudo halt 立即关机  

sudo reboot 
sudo init 6 重启命令 

ctrl+alt+f1~f6 从图形界面 切换到  命令行   
		f7   从命令行切换回 图形界面 
		
    
  
```

| 快捷键             | 作用                   |
| ------------------ | ---------------------- |
| tab                | 自动补全快捷键         |
| ctrl+c             | 立即终止正在运行的命令 |
| ctrl+a             | 回到命令的开头         |
| ctrl+e             | 回到命令的结尾         |
| ctrl+u             | 清除命令行             |
| clear  或者 ctrl+l | 清屏                   |

## 如果命令执行不成功   原因  

* 命令不存在   

* 多了 或者少了空格  都不对  比如 poweroff  多空格就执行失败  

* 命令没有安装   导致执行不成功

* linux 严格区分大小写    比如 sudo 正确  SUDO 失败

  

  

     

## 学习操作系统  必备工具   手册   帮助文件  

```
命令  --help 查看命令的帮助文件   每个命令后面 会有好多参数 
sudo apt-get mandb  
man ls  也是查看 ls 的帮助  man 命令  查看命令的帮助文件     q 退出     
sudo super user do 以超级管理员的身份运行  
. 开头的文件表示 隐藏文件    


```

## 命令提示符  

```
jinxingping@jinxingping-Parallels-Virtual-Platform:~$
jinxingping 用户名  
jinxingping-Parallels-Virtual-Platform 主机名   hostname    
hostname  获取主机名   
hostname 新名字 设置主机名  注销以后在登陆  就显示新的名字  
~  jinxingping 用户的家目录   root 用户的家目录 是/root    一般用户的家目录是 /home/用户名
~ /home/jinxingping  指的是一个目录  
$ 普通用户正在输入  
# 管理员用户正在输入  

```

## 软件安装   

```
sudo  apt-get install 软件名 安装软件  
duso  apt-get remove  软件名  删除软件 
sudo apt-get source 包名 查看 软件的源代码  
sudo apt-cache showsrc tree 查看该软件的 软件包信息
sudo apt-get update 获取最新的软件包列表   
sudo apt-get upgrade 更新可以更新的软件包  


tar -zxvf .tar.gz的压缩包  解压压缩包    
```

## Ubuntu下面安装 pycharm  

> 下载地址 : http://www.jetbrains.com/pycharm/download/#section=linux

##　命令行安装

```
sudo snap install pycharm-professional --classic
```

　

## 安装搜狗输入法   

```shell
http://cdn2.ime.sogou.com/dl/index/1524572264/sogoupinyin_2.2.0.0108_amd64.deb?st=s18fv4R-hpWaGlu4cTMWaQ&e=1531815848&fn=sogoupinyin_2.2.0.0108_amd64.deb


##添加源 
	sudo add-apt-repository ppa:fcitx-team/nightly
	sudo apt-get update 
	sudo apt-get -f install 
    sudo apt-get install fcitx
	sudo apt-get install fcitx-config-gtk 
	sudo apt-get install fcitx-table-all  
	sudo apt-get install im-switch
	#切换到sogoupinyin_2.2.0.0108_amd64.deb所在的目录
	sudo dpkg -i sogoupinyin_2.2.0.0108_amd64.deb
	系统设置-语言->键盘输入法 设置成 fcitx 重启即可  
```

