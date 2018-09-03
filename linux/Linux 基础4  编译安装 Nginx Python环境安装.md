## Linux 基础4  编译安装 Nginx Python环境安装   

## 复习   

```

```

## 编译安装的步骤

* 解压并进入目录  

* ./configure  --prefix=/usr/local/apache   --with 配置过程  --profix  将 apache 安装在 /usr/local/apache2下面   --with  --enable --disable 

* make  编译   

* make install   安装 

* make &&　make install   

  

## 编译安装Apache  

> 安装 依赖   
>
> sudo apt-get install gcc   libexpat1-dev  libpcre3 libpcre3-dev openssl libssl-dev 

1. 下载软件   

   ```
   https://mirror.bit.edu.cn/apache/httpd/httpd-2.4.34.tar.gz  apache  
   https://mirror.tuna.tsinghua.edu.cn/apache/apr/apr-1.6.3.tar.gz   apr  
   https://mirror.tuna.tsinghua.edu.cn/apache/apr/apr-util-1.6.1.tar.gz apr-util 
   ```

2 . 安装apr

```
tar -zxvf apr-1.6.3.tar.gz 
cd apr-1.6.3  解压进入目录   
./configure  --prefix=/usr/local/apr 
sudo make 
sudo make install 
```

3.安装 apr-util  

```
tar -zxvf apr-util-1.6.1.tar.gz
cd apr-util-1.6.1
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr
sudo make 
sudo make install
```

3.安装 Apache  

```shell
tar -zxvf httpd-2.4.34.tar.gz
cd httpd-2.4.34
./configure 
	--prefix=/usr/local/httpd2.4  #指定安装在哪里
    --sysconfdir=/etc/httpd2.4  #配置文件放到哪里
    --enable-ssl   #启用安全连接选项
    --with-pcre   #依赖于pcre 
    --with-apr-util=/usr/local/apr-util   #需要依赖apr-util
sudo make  编译
sudo make install  安装 
```

4.启动服务  

```shell
切换到Apache安装目录  
cd /usr/local/httpd2.4/bin
./apachectl start

会出现错误 
cd /etc/httpd2.4 #切换到配置文件所在的目录  

查找  /ServerName  
:set nu  
:194  


ServerName localhost:80
DocumentRoot "/usr/local/httpd2.4/htdocs"  #网站的根目录 换句话说就是  你写的代码要放到这里别人才能访问 

cd /usr/local/httpd2.4/bin
sudo ./apachectl start
```



## 开机启动   

```
查看哪些软件开机启动   

sudo apt-get install sysv-rc-conf    

sysv-rc-conf  

如果新安装软件 让他开机启动   就是选中2 3  4  5  因为2345 都是 完全多用户图形界面  一个级别 
空格选中  空格取消   
如果 取消 某个开机启动 只需要将其  2345 为空即可   

新建一个脚本  想要  开机的时候 自动执行这个脚本   

cd  /etc/init.d/

vim test 

#!/bin/bash 

echo '' >> /text.txt


chmod +x test  


update-rc.d /etc/init.d/test defaults 100 值越大 优先级越低  执行的越晚 



```

 ## 防火墙   

```
最开始 是防止大楼着火 现在广泛应用于 防止入侵   


系统 有 0~65535 个端口  


0~128 已经被占用 

用户可以自定义端口

129以后的   

1000以下的端口 需要 管理员的权限  


安装: 
	sudo apt-get install ufw  

防火墙常见操作:
	sudo ufw status  查看目前防火墙的状态  
	sudo ufw enable 启用防火墙  
	sudo ufw disable 关闭防火墙  
	sudo ufw reset 重置防火墙
	sudo ufw status numbered 查看规则的编号    
设置默认策略: 
	sudo ufw default deny incoming # 拒绝所有的传入连接  
	sudo ufw default allow outgoing #允许所有的传出连接  
	
开启或者关闭一些常用的连接  
	sudo ufw allow http #允许连接   80
	sudo ufw allow https #允许连接 443
	sudo ufw allow ftp #允许连接 21
	sudo ufw allow ssh #允许连接 22
	sudo ufw allow mysql #允许连接 3306
允许特定范围的端口号   
	sudo ufw allow 2000:3000/tcp 允许通过tcp 访问 2000-3000的端口  
允许特定的ip地址访问  
	sudo ufw allow from 10.8.152.88 
允许特定的网段访问   255.255.255.0 
	sudo ufw allow from 10.8.152.0/24    #允许10.8.152.1-10.8.152.254 访问 
拒绝连接 
	sudo ufw deny http 
	sudo ufw deny from 10.8.152.44 
```



## 删除规则    

```
sudo ufw status numbered 
sudo ufw delete 2 删除第二条规则  

sudo ufw delete allow http 
sudo ufw delete allow 80 
```

## nginx 安装   



## 安装 Python   

pyenv 这个工具用来  管理我们的Python版本    

virtualenv  管理不同的环境   



### pyenv  如果以前已经安装了 python2.7 3.5 pyenv 管理不了 需要管理的话我们得删除原来的版本  重新用pyenv 来安装    

> https://github.com/pyenv/pyenv-installer

```
如果没有安装vim  先sudo apt-get install vim 
sudo apt-get install curl 
sudo apt-get install git 
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash
或者 git clone  https://github.com/pyenv/pyenv-installer.git

将这三条写入.bashrc    vim /home/你的用户名/.bashrc  

export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

soucre /home/xyf/.bashrc   #让配置文件立即生效   


查看是否安装成功   
echo $PATH;
/home/xyf/.pyenv/plugins/pyenv-virtualenv/shims:/home/xyf/.pyenv/shims:~/.pyenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

如果出现 shims 说明成功 


sudo pyenv update
```

### 使用pyenv python 版本管理器    

```
pyenv install --list   列出 pyenv 支持的 Python版本   

pyenv versions 列出可用的Python版本   

```



## 安装Python  

````
cd /var/lib/dpkg && sudo mv info info.bak && sudo mkdir info
1.先安装 依赖包   
sudo apt-get install libc6-dev gcc  
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm

pyenv install 3.6.4 -v  安装很慢  
cd /home/你的用户名/.pyenv && sudo mkdir cache 
xyf@xyf-virtual-machine:~/.pyenv$ wget -c http://mirrors.sohu.com/python/3.6.4/Python-3.6.4.tar.xz -P /home/xyf/.pyenv/cache/ 
这么安装速度快 


wget -c http://mirrors.sohu.com/python/2.7.14/Python-2.7.14.tar.xz -P /home/jinxingping/.pyenv/cache/ 
pyenv install 2.7.14 -v  

-v 显示安装的过程  

xyf@xyf-virtual-machine:~/.pyenv$ wget -c http://mirrors.sohu.com/python/3.6.4/Python-3.6.4.tar.xz -P /home/xyf/.pyenv/cache/ 
pyenv install 3.6.4 -v 


更新 pyenv的数据库 
pyenv rehash 

查看 pyenv支持管理的python 版本   
pyenv versions  

选中3.6.4作为默认版本  
pyenv global 3.6.4  




````



 ## virtualenv   

> https://github.com/pyenv/pyenv-virtualenv 

如果有两个项目  a项目 和 b项目 同时使用 python 2.7.14 项目a 需要的flask 1.0 项目b 用的是flask2.0  

这个时候需要pyenv 和  virtualenv 结合来使用   

## 安装virtualenv  

如果你安装了 python3 以上的版本  会自动安装一个pip  pip 是python 的一个包管理工具  可以看成 windows 中的应用商店   可以管理软件 

```
pip install virtualenv 
sudo pip install --upgrade virtualenv 如果提示你版本不对 用这个命令  
pip install --upgrade pip
```



> 一个项目 一个 virtualenv   

```
sudo mkdir -p myproject/mall
 cd myproject/mall/
 
 创建项目的虚拟环境  
 
  pyenv virtualenv 3.6.4 env36    注意 : 必须是已经pyenv已经安装的版本  否则会报错
 
 切换到环境 
  pyenv activate env36 
(env36) jinxingping@jinxingping-Parallels-Virtual-Platform:~/.pyenv/myproject/mall$ 
表示 你现在处在 env36 环境中   

出来虚拟环境  
	


```





