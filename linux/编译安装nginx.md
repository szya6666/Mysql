# 编译安装nginx

## 安装依赖  

```shell
sudo apt-get install gcc automake autoconf libtool make build-essential libpcre3-dev 
```



## 下载软件

```shell
wget -c https://ftp.pcre.org/pub/pcre/pcre-8.42.tar.gz
wget -c https://www.openssl.org/source/openssl-1.0.2h.tar.gz
wget -c http://zlib.net/zlib-1.2.11.tar.gz
wget -c http://nginx.org/download/nginx-1.9.9.tar.gz

放到 /tmp目录下  
```

## 分别解压  

```shell
tar -zxvf pcre-8.42.tar.gz
tar -zxvf openssl-1.0.2h.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
tar -zxvf nginx-1.9.9.tar.gz
```

## 进入nginx-1.9.9目录 

```shell
cd nginx-1.9.9

./configure 
	--prefix=/usr/local/nginx  #安装路径
    --pid-path=/usr/local/nginx/logs/nginx.pid#进程文件
    --error-log-path=/usr/local/nginx/logs/error.log #错误日志路径
    --http-log-path=/usr/local/nginx/logs/access.log #访问日志路径
    --with-http_ssl_module # 启用http ssl 安全访问模块
    --with-pcre=/tmp/pcre-8.42 #依赖于pcre 模块
    --with-zlib=/tmp/zlib-1.2.11 #依赖于zlib模块
    --with-openssl=/tmp/openssl-1.0.2h #依赖于openssl模块

sudo make 

sudo make install


sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

sudo ln -s /usr/local/nginx/sbin/nginx /usr/bin/nginx   
直接nginx就可以启动了   

```

## 配置 service nginx restart 开机启动   

> sudo vim /etc/init.d/nginx  
>
> 进入编辑模式 然后复制下面的内容 并粘贴 注意复制完全  如果有格式 可以放到 txt中然后再复制  

```shell
------------------------------复制下面开始-------------------------------------
#!/bin/sh

### BEGIN INIT INFO
# Provides:      nginx
# Required-Start:    $local_fs $remote_fs $network $syslog $named
# Required-Stop:     $local_fs $remote_fs $network $syslog $named
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the nginx web server
# Description:       starts nginx using start-stop-daemon
### END INIT INFO

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DAEMON=/usr/local/nginx/sbin/nginx
NAME=nginx
DESC=nginx

# Include nginx defaults if available
if [ -r /etc/default/nginx ]; then
    . /etc/default/nginx
fi

STOP_SCHEDULE="${STOP_SCHEDULE:-QUIT/5/TERM/5/KILL/5}"

test -x $DAEMON || exit 0

. /lib/init/vars.sh
. /lib/lsb/init-functions

# Try to extract nginx pidfile
PID=$(cat /usr/local/nginx/conf/nginx.conf | grep -Ev '^\s*#' | awk 'BEGIN { RS="[;{}]" } { if ($1 == "pid") print $2 }' | head -n1)
if [ -z "$PID" ]; then
    PID=/run/nginx.pid
fi

if [ -n "$ULIMIT" ]; then
    # Set ulimit if it is set in /etc/default/nginx
    ulimit $ULIMIT
fi

start_nginx() {
    # Start the daemon/service
    #
    # Returns:
    #   0 if daemon has been started
    #   1 if daemon was already running
    #   2 if daemon could not be started
    start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON --test > /dev/null \
        || return 1
    start-stop-daemon --start --quiet --pidfile $PID --exec $DAEMON -- \
        $DAEMON_OPTS 2>/dev/null \
        || return 2
}

test_config() {
    # Test the nginx configuration
    $DAEMON -t $DAEMON_OPTS >/dev/null 2>&1
}

stop_nginx() {
    # Stops the daemon/service
    #
    # Return
    #   0 if daemon has been stopped
    #   1 if daemon was already stopped
    #   2 if daemon could not be stopped
    #   other if a failure occurred
    start-stop-daemon --stop --quiet --retry=$STOP_SCHEDULE --pidfile $PID --name $NAME
    RETVAL="$?"
    sleep 1
    return "$RETVAL"
}

reload_nginx() {
    # Function that sends a SIGHUP to the daemon/service
    start-stop-daemon --stop --signal HUP --quiet --pidfile $PID --name $NAME
    return 0
}

rotate_logs() {
    # Rotate log files
    start-stop-daemon --stop --signal USR1 --quiet --pidfile $PID --name $NAME
    return 0
}

upgrade_nginx() {
    # Online upgrade nginx executable
    # http://nginx.org/en/docs/control.html
    #
    # Return
    #   0 if nginx has been successfully upgraded
    #   1 if nginx is not running
    #   2 if the pid files were not created on time
    #   3 if the old master could not be killed
    if start-stop-daemon --stop --signal USR2 --quiet --pidfile $PID --name $NAME; then
        # Wait for both old and new master to write their pid file
        while [ ! -s "${PID}.oldbin" ] || [ ! -s "${PID}" ]; do
            cnt=`expr $cnt + 1`
            if [ $cnt -gt 10 ]; then
                return 2
            fi
            sleep 1
        done
        # Everything is ready, gracefully stop the old master
        if start-stop-daemon --stop --signal QUIT --quiet --pidfile "${PID}.oldbin" --name $NAME; then
            return 0
        else
            return 3
        fi
    else
        return 1
    fi
}

case "$1" in
    start)
        log_daemon_msg "Starting $DESC" "$NAME"
        start_nginx
        case "$?" in
            0|1) log_end_msg 0 ;;
            2)   log_end_msg 1 ;;
        esac
        ;;
    stop)
        log_daemon_msg "Stopping $DESC" "$NAME"
        stop_nginx
        case "$?" in
            0|1) log_end_msg 0 ;;
            2)   log_end_msg 1 ;;
        esac
        ;;
    restart)
        log_daemon_msg "Restarting $DESC" "$NAME"

        # Check configuration before stopping nginx
        if ! test_config; then
            log_end_msg 1 # Configuration error
            exit $?
        fi

        stop_nginx
        case "$?" in
            0|1)
                start_nginx
                case "$?" in
                    0) log_end_msg 0 ;;
                    1) log_end_msg 1 ;; # Old process is still running
                    *) log_end_msg 1 ;; # Failed to start
                esac
                ;;
            *)
                # Failed to stop
                log_end_msg 1
                ;;
        esac
        ;;
    reload|force-reload)
        log_daemon_msg "Reloading $DESC configuration" "$NAME"

        # Check configuration before stopping nginx
        #
        # This is not entirely correct since the on-disk nginx binary
        # may differ from the in-memory one, but that's not common.
        # We prefer to check the configuration and return an error
        # to the administrator.
        if ! test_config; then
            log_end_msg 1 # Configuration error
            exit $?
        fi

        reload_nginx
        log_end_msg $?
        ;;
    configtest|testconfig)
        log_daemon_msg "Testing $DESC configuration"
        test_config
        log_end_msg $?
        ;;
    status)
        status_of_proc -p $PID "$DAEMON" "$NAME" && exit 0 || exit $?
        ;;
    upgrade)
        log_daemon_msg "Upgrading binary" "$NAME"
        upgrade_nginx
        log_end_msg $?
        ;;
    rotate)
        log_daemon_msg "Re-opening $DESC log files" "$NAME"
        rotate_logs
        log_end_msg $?
        ;;
    *)
        echo "Usage: $NAME {start|stop|restart|reload|force-reload|status|configtest|rotate|upgrade}" >&2
        exit 3
        ;;
esac

---------------------结束 ---------------------------------------------
```

### 赋予执行权限 

```
#设置服务脚本有执行权限
sudo chmod +x /etc/init.d/nginx
#注册服务
cd /etc/init.d/
sudo update-rc.d nginx defaults
```



### 常用命令

```shell
sudo service nginx {start|stop|restart|reload|force-reload|status|upgrade}
```



## apt-get 安装 出现错误

```
有的时候，在Ubuntu下使用sudo apt-get install可能导致意想不到的错误，尤其是中途中断了安装时，错误信息为：

Errors were encountered while processing:

/var/cache/apt/archives/shotwell_0.18.0-1~saucy1-i386.deb

E: Sub-process /usr/bin/dpkg returned an error code (1)



此时可以这样解决：

cd /var/lib/dpkg

sudo mv info info.bak

sudo mkdir info



重新安装，在此为：

sudo apt-get install shotwell

以下未经测试：

我就是这样解决的 拿来给你分享一下～～～～

在处理时有错误发生： 

ttf-opensymbol 

E: Sub-process /usr/bin/dpkg returned an error code (1)



在处理时有错误发生： 

ttf-opensymbol 

E: Sub-process /usr/bin/dpkg returned an error code (1)

解决方案代码：

sudo fc-cache -fv 2>&1 | grep failed | cut -f1 -d":" | xargs -i sudo touch {} && sudo fc-cache -fv

```

## 配置vhost    

```
cd /usr/local/nginx/conf   #nginx 配置文件目录 
vim nginx.conf    

倒数第二行 加入  
include /usr/local/nginx/conf/vhost/*.conf; #不要忘了分号   

mkdir -p /usr/local/nginx/conf/vhost
mkdir -p /usr/local/nginx/html/baidu 

	cd /usr/local/nginx/html/baidu 
	 vim index.html 
	 	baidu
mkdir -p /usr/local/nginx/html/google
	 cd /usr/local/nginx/html/google
	 vim index.html 
	 	google
cd /usr/local/nginx/conf/vhost

vim  www.baidu.com.conf  

server{
    listen 80;
    server_name www.baidu.com baidu.com;

    root /usr/local/nginx/html/baidu;




}

vim  www.google.com.conf  

server{
    listen 80;
    server_name www.google.com google.com;

    root /usr/local/nginx/html/google;




}

保存 


重启nginx   
	service nginx restart  
	ps -aux | grep nginx  
	kill -9   
	nginx -c /usr/local/nginx/conf/nginx.conf 
	
	
到 windows下面   C:\Windows\System32\drivers\etc  

编辑hosts   
   添加  你的ip地址  www.baidu.com
   		你的ip地址  www.google.com
   		
   		
 接下来  打开两个窗口 分别 baidu google  就会看到不同的内容  
```



