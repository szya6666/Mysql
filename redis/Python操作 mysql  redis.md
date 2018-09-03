# Python操作 mysql  redis  

## 内存  

* 栈内存 
  * 基本数据类型 string  number bool undefined  null 
* 堆内存 
  * 引用数据类型  object  function  



##　创建表

```python
create table if not exists 表名()engine=  default charset=utf8;

#!/root/.pyenv/shims/python

import pymysql 
db= pymysql.connect('127.0.0.1','root','123456','python1805')

cursor = db.cursor()
#创建表之前 先要判断 表是否存在  如果存在 删除

sql = "create table if not exists kangbazi(id int(11) unsigned primary key not null,name varchar(32) not null)engine=myisam default charset=utf8"

cursor.execute(sql)


cursor.close()
db.close()

python created.py
```

 ## 更新数据  

```python 
#!/root/.pyenv/shims/python

import pymysql 
db= pymysql.connect('127.0.0.1','root','123456','python1805')

cursor = db.cursor()

sql = 'update user set name="回头是苏乾" where id=2'
try:
    cursor.execute(sql)
    db.commit()
except:
    db.rollback()
cursor.close()
db.close()

```

## 插入数据 

```python
#!/root/.pyenv/shims/python

import pymysql 
db= pymysql.connect('127.0.0.1','root','123456','python1805')

cursor = db.cursor()
sql = "insert into kangbazi values(1,'苦海无涯'),(2,'回头是我'),(3,'可爱不是长久之计'),(4,'爱我是长久之计')"

try:
    cursor.execute(sql)
    db.commit()
except:
    db.rollback


cursor.close()
db.close()
```

## 删除数据 

```python
#!/root/.pyenv/shims/python

import pymysql 
db= pymysql.connect('127.0.0.1','root','123456','python1805')

cursor = db.cursor()

sql = 'delete from kangbazi where id=2'
try:
    cursor.execute(sql)
    db.commit()
except:
    db.rollback()
cursor.close()
db.close()

```

## 查询数据  

```python
#!/root/.pyenv/shims/python

import pymysql
db= pymysql.connect('127.0.0.1','root','123456','python1805')

cursor = db.cursor()

sql = 'select * from kangbazi'
try:
    cursor.execute(sql)
    res = cursor.fetchall()
    for row in res:
        print('%d~%s' %(row[0],row[1]) )
except:
    db.rollback()

cursor.close()
db.close()


```

wget 递归下载 别人的网站  只能拿前台 页面 

```
wget -r -k -np -p -nc https://www.douban.com    
-r 递归下载 
-k 将远程连接转化为本地连接 
-np 不追溯到父级
-nc 断点续传   
-p 页面的必须元素  下载下来  
```



# Redis  

> nosql  非关系型数据库   在内存里操作  速度快   高性能的key-value 数据库  
>
> 开源 免费  
>
> 项目中经常用redis 来给mysql 做缓存   聊天室  数据推送 
>
> 单个的key  可以达到512M  
>
> redis 单线程    
>
> 支持集群  
>
> 有16个库  0~15号 

### 列出你知道的 nosql  

* redis  
* mongodb  
* couchdb
* memcached
* rabbitMQ  消息队列  

## 官网  https://redis.io/   http://www.redis.cn/

编译安装软件  

* ./configure --prefix=     --with   --enable 
* make 
* make install 

### 安装redis  

```shell
wget -c http://download.redis.io/releases/redis-3.2.9.tar.gz  先下载
tar -zxvf redis-3.2.9.tar.gz  解压 
mv redis-3.2.9 /usr/local/redis  移动目录

cd /usr/local/redis   切换到移动后的目录 
make install  即可  直接安装 


```

### 启动redis  默认端口号  6379   

./表示执行

```shell
cd /usr/local/redis/src 
./redis-server  这个时候 发现服务前台启动没法进行其它工作  

让redis 后台启动 
vim /usr/local/redis/redis.conf  

daemonize yes 将 no 改为yes   后台启动 
保存退出   该配置文件  记住 先备份  

/usr/local/redis/src/redis-server /usr/local/redis/redis.conf  启动的时候加上配置文件 表示 以配置文件方式启动 这样配置文件会生效  

查看是否启动 
ps -aux | grep redis 
netstat -ntlp | grep 6379



```



虽然说 redis 在内存中操作  在内存中 重启数据容易丢失  aof rdb 可以让redis  在内存中的数据持久化  不丢失 

```
cd /usr/local/redis/src 
```

| 文件名称        | 作用                         |
| --------------- | ---------------------------- |
| redis-server    | redis 服务器                 |
| redis-cli       | redis客户端                  |
| redis-check-aof | aof检测工具                  |
| redis-check-rdb | rdb检测工具                  |
| redis-benchmark | 哨兵服务器 2.0之后出现了这个 |

### redis 持久化机制  

* aof
* rdb 



### 配置redis 开机启动   

开机启动项 会到  /etc/rc.local 查找    

```shell
cd /usr/local/redis/utils/
redis_init_script 是redis 初始化启动脚本     
vim redis_init_script 

REDISPORT=6380  你在redis.conf中设置的端口号
EXEC=/usr/local/redis/src/redis-server  redis-server 所在的路径
CLIEXEC=/usr/local/resis/src/redis-cli  redis-cli 所在的路径 

PIDFILE=/var/run/redis_${REDISPORT}.pid  进程文件保存的路径  redis_6380.pid
CONF="/usr/local/redis/redis_${REDISPORT}.conf"    redis_6380.conf  redis 配置文件所在的路径  

接着到 
cd /usr/local/redis
cp redis.conf  redis_6380.conf 要不然redis_init_script  找不到 

然后再   vim /etc/rc.local  
写入 这一行  
/usr/local/redis/utils/redis_init_script start   


然后开机 就会启动redis 
```

### 连接redis

```shell
redis-cli -h  ip地址 -p 端口号    

ping 返回  PONG 说明连接成功 

第一次登陆 没有密码 
设置密码   
config set requirepass kangbazi  
接下来重新登陆  
redis-cli -h 127.0.0.1 -p 6380 
会出现  （error) NOAUTH Authentication required 提示你输入密码  

auth kangbazi   
ping 
PONG 说明登陆成功 
```



** redis 有五种数据类型不止5种 常用的 5种 重点  ** 

* string 
* hash
* list 
* set 
* zset    

#### string 最基本的数据类型  key value   二进制 安全 也就是说可以存任何数据  图像音频 

一个key 最大 512m 

```
set 往redis 中存数据  

set key value 
set name kangbazi  
name 就是 key 
kangbazi 就是 value 

------------------------------
del  key 删除指定key 中的内容
---------------------------------
127.0.0.1:6380> mset banzhang qiqi gaofeng xiaoding jingqiang xiaoqiang   批量设置多个
OK
127.0.0.1:6380> get banzhang
"qiqi"
127.0.0.1:6380> get gaofeng 
"xiaoding"
127.0.0.1:6380> get jingqiang 
---------------------------------------
127.0.0.1:6380> setex qiqi 20 xiaowang  设置 qiqi 这个key 的过期时间  秒为单位 -1 表示永不过期
OK

--------------------------------------------------------------------
127.0.0.1:6380> mget banzhang gaofeng jingqiang   批量 取多个key的值 
1) "qiqi"
2) "xiaoding"
3) "xiaoqiang"
----------------------------------------------------------------------
set num 1 
incr key   让指定的key的value 累加1  
decr key   让指定的key的value 累减1 
incrby key 指定的整型数字 让指定的key的value 累加指定的值
decrby key  指定的整型数字 让指定的key的value 累减指定的值
-----------------------------------------------------------------------
127.0.0.1:6380> set num 1805
OK
127.0.0.1:6380> append num kangbazi 在指定的key 后面 拼接指定的value 值  
(integer) 12
127.0.0.1:6380> get num
-----------------------------------------------------------------------
strlen key  获取指定key 的值的长度   

127.0.0.1:6380> get num
"1805kangbazi"
127.0.0.1:6380> strlen num 
(integer) 12

```



## key 的操作  

```
select 0~15 选择0-15号库 
keys * 列出该库中所有的key 
keys a* 列出库中a开头的key
当时没设置过期时间  可以使用 
expire key 秒     -1 永不过期   
exists key  查看key 是否存在  存在 返回1  不存在返回0 
ttl key 查看该key的过期时间   
type key 查看key的类型    


flushdb 清空当前库中 所有的key  
flushall 清空 所有库中的key  
```



### hash 键值对的集合 主要用来存储对象  一般用hash 类型做缓存     

一张表 一个hash  

每一个hash 可以存放 2的32次方-1个键值对  

```
var zhanghua = 
{
    name:'zhanghuang',
    age:18,
    talk:function(){
        
    }
    
}

hset key 字段名 字段值 
	127.0.0.1:6380> hset person name zhanghuang
    (integer) 1
    127.0.0.1:6380> hset person age 18
    (integer) 1
hget key 字段名 
	127.0.0.1:6380> hget person name 
    "zhanghuang"
    127.0.0.1:6380> hget person age
    "18"
    127.0.0.1:6380> hget person height 
    "181cm"
hmset people name gaofeng age 18 height 181cm weight 90kg   批量设置 
hmget people name age height weight   批量获取  
1) "gaofeng"
2) "18"
3) "181cm"
4) "90kg"

hgetall people   跟据 key 获取所有   里边包含 属性名 和属性的值 
1) "name"
2) "gaofeng"
3) "age"
4) "18"
5) "height"
6) "181cm"
7) "weight"
8) "90kg"

hkeys people   根据key 获取全部的属性
1) "name"
2) "age"
3) "height"
4) "weight"
 hvals people   根据key 获取全部的属性的值
1) "gaofeng"
2) "18"
3) "181cm"
4) "90kg"


hdel people weight  删除指定key中指定属性的值  
(integer) 1
hstrlen people height 获取指定key中指定属性的属性值的长度    181cm 返回5
(integer) 5
hexists people weight  查看指定key的属性是否存在 存在返回1 不存在返回 0  
(integer) 0


123 用来区分数据行    可以使用 incr 自增    
127.0.0.1:6380> hmset user:1 name canglaoshi age 35 province dongjing 
OK
127.0.0.1:6380> hmset user:2 name bolaoshi age 30 province xijing 
OK
127.0.0.1:6380> hmset user:3 name fanlaoshi age 25 province nanjing 

```



## list  队列  元素类型为string 在列表的头部或者尾部  插入元素     

```
127.0.0.1:6380> lpush sql mysql 
(integer) 1
127.0.0.1:6380> lpush sql sqlserver 
(integer) 2
127.0.0.1:6380> lpush sql oracle
(integer) 3
127.0.0.1:6380> rpush sql redis
(integer) 4
127.0.0.1:6380> rpush sql mongodb
(integer) 5
127.0.0.1:6380> rpush sql couchdb
(integer) 6
127.0.0.1:6380> lrange sql 0 5   获取元素  lrange key  start stop  从0开始 
1) "oracle"
2) "sqlserver"
3) "mysql"
4) "redis"
5) "mongodb"
6) "couchdb"

lset key 指定索引值 新内容 
127.0.0.1:6380> lset sql 2 mysql666 更新指定key 对应的 索引值的 value 
127.0.0.1:6380> lrange sql 0 5
1) "oracle"
2) "sqlserver"
3) "mysql666"
4) "redis"
5) "mongodb"
6) "couchdb"


127.0.0.1:6380> linsert sql before redis rabbit 在key 对应值 中 指定的值前面 添加 一个值 
(integer) 7

127.0.0.1:6380> lrange sql 0 6
1) "oracle"
2) "sqlserver"
3) "mysql666"
4) "rabbit"
5) "redis"
6) "mongodb"
7) "couchdb" 



127.0.0.1:6380> lpop sql  从开头弹出第一个 
"oracle"
127.0.0.1:6380> rpop sql  从结尾弹出第一个 
"couchdb"



2) "sqlserver"
3) "mysql666"
4) "rabbit"
5) "redis"
6) "mongodb"

127.0.0.1:6380> ltrim sql 1 3    包括 第一和第三  截取指定范围的值 替换掉原来的 
OK
127.0.0.1:6380> lrange sql 0 6
1) "mysql666"
2) "rabbit"
3) "redis"


127.0.0.1:6380> llen sql   查看队列中 值的个数   
(integer) 3
 127.0.0.1:6380> lindex sql 2  查看队列中 指定的key 对应的索引的值   2是索引 的值 索引从0开始算  
"redis"

```



## set  无序集合  元素类型 也是string  元素具有唯一性  不能重复     

```
添加元素  
127.0.0.1:6380> sadd 1805 kangbazi 
(integer) 1    存入成功  1 
127.0.0.1:6380> sadd 1805 kangbazi
(integer) 0   已经存在   忽略 返回0 


127.0.0.1:6380> sadd miaosha jingqiang
(integer) 1
127.0.0.1:6380> sadd miaosha saixin
(integer) 1
---------------------------------
列出元素   

127.0.0.1:6380> smembers miaosha 列出 key 集合中所有的元素
1) "saixin"
2) "peiqing"
3) "gaofeng"
4) "jingqiang"
127.0.0.1:6380> sadd miaosha saixin 如果存在返回0 
(integer) 0
-------------------------------------------------
127.0.0.1:6380> scard miaosha  查看集合元素的个数   
(integer) 4

----------------------------------------
查看多个key 的交集 

127.0.0.1:6380> smembers miaosha
1) "jingqiang"
2) "gaofeng"
3) "peiqing"
4) "saixin"
127.0.0.1:6380> sadd kangbazi gaofeng
(integer) 1
127.0.0.1:6380> sadd kangbazi zaoyang 
(integer) 1
127.0.0.1:6380> sadd kangbazi jingqiang 
(integer) 1
127.0.0.1:6380> sinter miaosha kangbazi 
1) "gaofeng"
2) "jingqiang"
---------------------------------
查看多个key 之间的差集  
127.0.0.1:6380> sdiff miaosha kangbazi 
1) "saixin"
2) "peiqing"
-----------------------------------
查看多个key 之间的合集 
127.0.0.1:6380> sunion miaosha  kangbazi 
1) "jingqiang"
2) "gaofeng"
3) "peiqing"
4) "saixin"
5) "zaoyang"
-------------------------------------
查看集合中元素是否存在    s  is member 
127.0.0.1:6380> sismember miaosha saixin  查看saixin元素是否在  miaosha集合中  
(integer) 1

```



## zset string 类型的有序集合 元素具有唯一性  不能重复      

```
添加元素   
				   key     分数  元素  
127.0.0.1:6380> zadd numbers 0 mysql
(integer) 1
127.0.0.1:6380> zadd numbers 0 sqlserver 
(integer) 1
127.0.0.1:6380> zadd numbers 0 oracle
(integer) 1
127.0.0.1:6380> zadd numbers 1 redis
(integer) 1
127.0.0.1:6380> zadd numbers 1 mongodb
(integer) 1
127.0.0.1:6380> zadd numbers 2 memcache
(integer) 1
127.0.0.1:6380> zadd numbers 3 rabbitMQ 

获取元素  
	127.0.0.1:6380> zrange numbers 0 6 从 第0个位置开始 取到第6个位置  包含第0个和第6个位置   
    1) "mysql"
    2) "oracle"
    3) "sqlserver"
    4) "mongodb"
    5) "redis"
    6) "memcache"
    7) "rabbitMQ"
127.0.0.1:6380> zrangebyscore numbers 2 3  根据分数获取   取出 分数为 2 到3 对应的元素包含2和3 
1) "memcache"
2) "rabbitMQ"

  flag =0  incr(flag )
  zadd pythonmiaosha flag 每个人的手机号
  
  
 返回指定的 key 中元素的个数   
 127.0.0.1:6380> zcard  numbers  numbers集合中有七个元素    
(integer) 7

127.0.0.1:6380> zcount  numbers 0 1 查看指定分数区间内的元素   
(integer) 5
127.0.0.1:6380> zcount  numbers 2 3
(integer) 2

查看指定 元素对应的分数    添加"rabbitMQ"的时候分数是3  
127.0.0.1:6380> zscore numbers rabbitMQ
"3"

```

## redis 主从   

| IP地址      | 角色            |
| ----------- | --------------- |
| 10.11.68.57 | master  6380    |
| 10.11.68.57 | slave      6379 |

【步骤】

  准备工作 : 两台主机分别关闭防火墙   

* sudo ufw disable 

1. 先在主服务器上进行操作  

  ```

vim /usr/local/redis/redis.conf 

bind 0.0.0.0 将127.0.0.1 改为 0.0.0.0 

重启redis   

ps -aux | grep redis  
kill -9

或者说  redis-cli -h 127.0.0.1 -p 6380 -a kangbazi shutdown   
ps -aux | grep redis  
kill -9

再启动  
/usr/local/redis/utils/redis_init_scripts start 
重启后设置密码  
  ```



2.在从库上进行操作  为了跟 主库区别开来 端口号不跟主库一样

```
vim /usr/local/redis/redis.conf  

port 6379 


：set nu  
第265行 首先去掉前面 #注释 
slaveof 10.11.68.57 6380
		主redis ip地址 主redis 的端口号
第272行   去掉注释 
masterauth kangbazi
		   主redis 设置的密码 
		   
重启redis  

ps -aux | grep redis  
kill -9 

/var/run/redis_6379.pid exists, process is already running or crashed
先cd /var/run 

rm -rf redis_6379.pid 删除 
再重启 
/usr/local/redis/utils/redis_init_script start
```



3.主从切换   

```
1.先让主redis 停掉 
redis-cli -h 127.0.0.1 -p 6380 -a 密码
127.0.0.1:6380> shutdown 
not connected> exit
root@alren-pc:~# redis-cli -h 127.0.0.1 -p 6380
Could not connect to Redis at 127.0.0.1:6380: Connection refused
Could not connect to Redis at 127.0.0.1:6380: Connection refused
not connected> 
2.将从redis 设置为主redis  
redis-cli -h 127.0.0.1 -p 6379 slaveof no one 不跟随任何的主库
3.修改 主redis的配置文件  
slaveof 10.11.68.18 6379
masterauth 密码   

重启  
```





### redis 持久化机制   

* rdb  全称 redis database
* aof   全程 append of file





#### rdb  

在指定的时间间隔内  将内存中的数据 以快照的形式  （跟虚拟机的快照一样 ）写入磁盘   

首先 redis-server redis服务器 定时创建子进程 （老板雇员工） 员工将内存中的数据 写入到临时文件dump.rdb中  这个文件会替换掉  原来的dump.rdb   

/usr/local/redis/src/dump.rdb   是一个二进制的文件  
/usr/local/redis/utils/dump.rdb

dump.rdb 的名称也可以修改  

```
vim /usr/local/redis/redis.conf  

第237行  
dbfilename dump.rdb

下面就是 时间间隔   
 202 save 900 1     900秒内 有一个key 被修改 发起快照保存 
 203 save 300 10    300秒内  有10个key 被修改  发起快照保存 
 204 save 60 10000  60秒内 有10000个key 被下修改 发起快照保存  
```

有点: 

​	非常适合备份  每个小时 保存过去24小时的数据  

​	fdb的操作是交给子进程来操作的  

​	恢复大数据  效率比aof 高   



### aof    append of file  

将redis-cli的命令 写入到文件中 下次重启 先执行文件中的命令  将数据恢复到内存中   



redis-cli 发送命令  到  redis-server   此时命令会写入到  appendonly.aof  

##### 开启aof  持久化机制  

```
vim /usr/local/redis/redis.conf  

593 appendonly yes 
第593行 将no 改为yes 
597 appendfilename "appendonly.aof"

重启 redis 


ps -aux | grep redis  

kill -9 进程号   


然后再启动  

注意 
/usr/local/redis/src/redis-server /usr/local/redis/redis.conf   

如果想  /usr/local/redis/utils/redis_init_script start  
你需要修改  redis_init_script 里边指定的配置文件   我指定的是 redis_6380.conf    
否则不起作用  

总结 ： 你改了哪个配置文件 下次启动要以这个配置文件启动  


成功之后 会在  /usr/local/redis/utils  看到一个 appendonly.aof  
```

##### aof 备份的频率 

```
# appendfsync always 只要有新命令追加  就执行一次同步  特别安全 但是慢 
appendfsync everysec 每秒同步一次   在速度和安全之间 同步做的最好  
# appendfsync no 从不同步   速度快 但是不安全  

```









