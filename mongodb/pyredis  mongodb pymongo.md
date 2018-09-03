# pyredis  mongodb pymongo

## redis复习  

redis-cli 

config set requirepass 

auth 密码

redis-cli -h ip -p 端口号 -a 密码 

/usr/local/redis/src/redis-server /usr/local/redis/redis.conf

/usr/local/redis/utils/redis_init_script start 

1. string  

   1. set 
   2. get 
   3. del 
   4. mset
   5. mget 
   6. incr 
   7. decr 
   8. incrby 
   9. decrby 
   10. incrbyfloat 
   11. append 
   12. strlen 
   13. keys * 
   14. keys a*
   15. type
   16. expire 
   17. ttl 
   18. exists  

2. hash  

   1. hset
   2. hget 
   3. hmset
   4. hmget 
   5. hgetall  获取属性和值 
   6. hkeys 获取属性  
   7. hvals 获取值 
   8. hlen 
   9. hexists  判断属性是否存在  
   10. hdel 删除属性  删除key value 也就没了  
   11. hstrlen  返回值的长度  

3. list 

   1. lpush
   2. rpush 
   3. lpop
   4. rpop
   5. lrange
   6. linsert  
   7. lset  设置指定索引的值  
   8. ltrim  
   9. llen
   10. lindex 

4. set 

   1. sadd 
   2. smembers 
   3. scard
   4. sismember

   

5. zset 

    zadd 

   zrange  key  start end  

   

redis 主从 

​    从库上  

  salveof IP地址 端口号  

  

## 命令行下 不用连接上redis  批量删除redis 中的key  

```
redis-cli -p 6380 keys "*" | xargs redis-cli -p 6380 del
redis-cli -h IP地址 -p 6380 keys "*" | xargs redis-cli -h IP地址  -p 6380 del 指定redis服务器及端口号

redis-cli -h IP地址 -n 1 -p 6380 keys "*" | xargs redis-cli -h IP地址  -n 1 -p 6380 del 指定1号库 及端口号  

redis-cli -h IP地址 -p 6380 keys "*" | xargs redis-cli -h IP地址  -p 6380 del
redis-cli -h IP地址 -p 6380 -a 密码 keys "*" | xargs redis-cli -h IP地址  -p 6380 -a 密码 del
```



## 安装redis 模块  

```
1.pip install redis 
 

2.git clone https://github.com/andymccurdy/redis-py.git 
cd redis-py 
sudo python3 setup.py install    用python3安装的redis 扩展



alren@alren-pc:~/redis-py$ python3   这里操作的时候 也得用python3版本 
Python 3.6.5 (default, Apr  1 2018, 05:46:30) 
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import redis    先引入redis 模块
>>> r=redis.StrictRedis(host='127.0.0.1',port=6380,db=0) 提供了两个方法 StrictRedis 和 Redis
>>> r.set('name','gaofeng')
True
>>> r.get('name')
b'gaofeng'  




```

> 默认情况下  设置的值或者取得的值 都是bytes 类型  如果向改成string 类型  需要加上decode_responses=True
>
> >> r=redis.StrictRedis(host='127.0.0.1',port=6380,db=0,decode_responses=True) 
> >> r.set('name','gaofeng')
> >> True
> >> r.get('name')
> >> 'gaofeng'   前面就没有 b 了   

vim redised.py

```python
#!/usr/bin/python3

#/home/alren/.pyenv/shicms/python  你用哪个python版本 安装的redis 扩展 这里你先用which找到命令的绝对路径 
#/root/.pyenv/shicms/python
#/usr/bin/python

import redis 
r = redis.StrictRedis(host='127.0.0.1',port=6380,db=0,decode_responses=True)
#r.set('kangbazi','1805')

#print(r.get('kangbazi'))

# 每一次 客户端到服务器 发送请求的时候  一次执行多条命令  这样会减少连接消耗 

pip = r.pipeline()
pip.set('name1','zhangsan')
pip.set('name2','lisi')
pip.set('name3','wangwu')
pip.execute()
print(r.get('name2'))

python3 redises.py
```

### python 操作redis string类型   

```
set
get 
setex(key,时间,value)
mset(name1='',name2='') name1 name2不能加单引号 
mget('name1','name2')  name1的值是 zhangsan
getrange(key,start,stop) 
getrange('name1',0,4) 返回的结果就是zhang  将name1对应的结果 进行截取从第0个位置 截取到第4个位置 
strlen('name1') 返回8  zhangsan的长度为 8 
incr('num1')
decr('num1')
incrbyfloat('num1',123.1)

```



vim cun.py

```
#!/usr/bin/python3

import redis 
r = redis.StrictRedis(host='127.0.0.1',port=6380,db=0,decode_responses=True)
#r.setex('names6',10,'values')
#r.mset(nam1='zhangsan',nam2='lisi')
#r.set('num1',20)

#r.hset('user_name','name','qiqi')
#arr = {"name":'gaofeng',"age":18,"height":'181cm',"weight":'80kg'}
#r.hmset("users",arr)

#r.lpush('lists',"mysql","sqlserver","oracle","memcache")

#r.sadd('set_user',"你可以帮我洗个东西么","洗什么呀","喜欢我啊")
r.zadd('zset_name',zhangsan=1,lisi=2,wangwu=3,pengyou=12,lianren=12,airen=12,jiaren=12,nanwang=520)

```



vim qu.py

```python
#!/usr/bin/python3

import redis 
r = redis.StrictRedis(host='127.0.0.1',port=6380,db=0,decode_responses=True)
#print(r.get('names6'))
#print(r.mget('nam1','nam2'))
#res = ['nam1','nam2']
#print(r.mget(res))

#print(r.getrange('nam1',0,4))
#print(r.strlen('nam1'))
#print(r.incr('num1'))
#r.append('num1','$')
#print(r.get('num1'))
#print(r.incrbyfloat('num1',20.5))
#print(r.decr('num1'))
#print(r.hget('user_name','name'))
#res = ["name","age","height","weight"]
#print(r.hmget('users',res))
#print(r.hgetall('users'))
#print(r.lrange('lists',0,-1))#-1表示取队列中的全部元素
#print(r.lindex('lists',2))
#print(r.rpop('lists'))
#print(r.smembers('set_user'))
#print(r.zrange('zset_name',0,-1))
#print(r.zrangebyscore('zset_name',12,520)) #跟据分数查看zset类型中的元素  
#print(r.zscore('zset_name','nanwang')) #查看指定key中元素的 分数
#print(r.exists('num1')) 
#print(r.exists('zset_name666')) #可以判断任何类型的key 是否存在 存在返回true  不存在返回false 
#r.delete('num1') #删除一个key 
#print(r.get('num1'))
#print(r.keys('*'))
#print(r.keys('n*'))
#r.set('user','peiqing')
#r.expire('user',10) #设置key的过期时间
#print(r.get('user'))
#r.set('lists','888889999')
#r.rename('lists','duilie')  #对key 进行重命名 
#print(r.get('duilie'))
#print(r.randomkey())  #随机从指定的库中 取出key名
#print(r.type('zset_name'))  #查看key的类型
r.move('zset_name',1) #将zset_name这个key 移动到1号库
print(r.zrange('zset_name',0,-1)) 
```



