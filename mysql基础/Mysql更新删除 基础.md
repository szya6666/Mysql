Mysql 基础

## mysql 组成部分  

* 数据库服务器    好比地球 人需要地球才能生存
* 数据库    一个Excel表格就是一个数据库 
* 数据表    Excel表格中的sheet 1 sheet2
* 数据字段  就是第一行的内容  就是table 的th
* 数据行  就是我们一行行的记录  table 的 tr td

## 常用端口号 

```
mysql 3306
ssh  22
scp  22
http 80
https 443
smtp 25
pop3 110 
ftp 21 
redis 6379
mongodb 27017 

系统有0~65535个端口号  Linux 中 1000以下的端口号 需要 root 权限  0~127 端口号 已经被占用  自定义端口号  在这个之外
```



## 连接数据库  

```
1.cmd
  mysql -u root -p 
  -u 后面跟用户名   
  -p 密码 
  
2.可视化连接  
	navicat连接 
	mysql端口号 3306 
```

## 数据库操作   

```
1.navicat 创建    右键 新建数据库
  字符集:utf8 -- UTF-8 Unicode 
  排序规则: utf8_general_ci

2.命令行 操作   
  mysql -u root -p
  


```

### 展示有哪些数据库 

```
  show databases; #别忘了加; 英文状态下的 ;号表示 这一句 结束了  没有;继续让你输入 直到遇到;

  mysql> show databases;

+--------------------+

| Database           |

+--------------------+

| information_schema |

| kangbazi           |

| kangbazi1805       |

| mysql              |

| performance_schema |

| python1804         |

| sys                |

+--------------------+

7 rows in set (0.00 sec)

```



### 创建数据库 

```
create 新数据库名字;
mysql> create database gaofeng;
Query OK, 1 row affected (0.00 sec)
```

## 选中数据库 

```
use 数据库名字;
mysql> use gaofeng;
Database changed  表示被选中 
```

### 查看数据库有哪些数据表 

```
show tables; 列出所有的数据表

mysql> show tables;
Empty set (0.00 sec)
```

### 删除数据库 

```
drop database 数据库名字;
mysql> drop database gaofeng;
Query OK, 0 rows affected (0.04 sec)
```

 



## 数据表操作  先选中数据库 然后查看 数据表    

### 创建数据表 长度可以不加 不加的话 会使用类型的默认长度  

```
create table 表名(字段名 类型(长度))；
mysql> create table python(id int(11) unsigned primary key auto_increment not null,name varchar(50) not null);

unsigned  表示 无符号  也就说  正数  
primary key 主键 
auto_increment 自增
not null 表示不为空 
int 整型 
varchar 字符型 

```

### 查看表结构 

```
mysql> desc python1805;
+-------+---------------------------+------+-----+---------+----------------+
| Field | Type                      | Null | Key | Default | Extra          |
+-------+---------------------------+------+-----+---------+----------------+
| id    | int(11) unsigned          | NO   | PRI | NULL    | auto_increment |
| name  | varchar(50)               | NO   |     |         |                |
| age   | int(11) unsigned zerofill | NO   |     | NULL    |                |
+-------+---------------------------+------+-----+---------+----------------+


show create table 表名\G \G后面可以不用写分号 
mysql> show create table python1805\G  以最佳的阅读体验来查看  创建表的过程 
*************************** 1. row ***************************
       Table: python1805
Create Table: CREATE TABLE `python1805` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL DEFAULT '' COMMENT '濮撳悕',
  `age` int(11) unsigned zerofill NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8
1 row in set (0.00 sec)
ENGINE  表示 mysql 引擎    
charset  表示字符集  


```

### 删除表   

```
drop table 表名；
mysql> drop table python1805;
Query OK, 0 rows affected (0.02 sec)
```



### 指定表mysql引擎 及 字符集  

```
引擎主要用两个: 
	myisam 如果说 你设这个这个表主要用来 插入数据 读取数据  引擎选择  myisam
	innodb  如果说 你设计这个表 用来 更新数据 删除数据   引擎选择 innodb 
	
mysql> create table python1805(id int(11) unsigned primary key auto_increment not null)engine=myisam default charset=utf8;
```



## 数据字段操作 alter table 表名  

### 修改表字段的类型

> alter table 表名 modify 字段名 类型（长度）；经常用    

```
mysql> desc python1805;
+-------+------------------+------+-----+---------+----------------+
| Field | Type             | Null | Key | Default | Extra          |
+-------+------------------+------+-----+---------+----------------+
| id    | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| name  | varchar(50)      | NO   |     | NULL    |                |
+-------+------------------+------+-----+---------+----------------+


mysql> alter table python1805 modify name char(64);
Query OK, 0 rows affected (0.02 sec)
```

### 添加表字段  

> alter table 表名  add column  字段名  类型(长度)；

```
mysql> alter table python1805 add column age int(11) not null;
Query OK, 0 rows affected (0.02 sec)
```

### 添加表字段的时候控制顺序  

> alter table 表名  add 字段名  类型(长度) after 字段名；

```
mysql> alter table python1805 add sex tinyint not null after name; 添加字段放到指定字段的后面
Query OK, 0 rows affected (0.02 sec)
desc python1805;


mysql> alter table python1805 add uid int(11) first;  添加字段放到 首位  
Query OK, 0 rows affected (0.03 sec) 
```



### 删除字段  

> alter table 表名 drop column 字段名；

```
mysql> alter table python1805 drop column uid;
Query OK, 0 rows affected (0.02 sec)
```



###　表字段改名

> alter table 表名 change 原来的字段名  字段新名字  类型（长度);  

```
mysql> alter table python1805 change name username varchar(32) not null;
Query OK, 0 rows affected (0.02 sec)
```

### 修改排序顺序  

> alter table 表 名 modify 字段名 类型(长度)  【not null】first ；

```
 alter table python1805 modify username varchar(32) first;
```



###　修改表名

```
alter table 旧表名 rename 新表名;

mysql> alter table python1805 rename python05;
Query OK, 0 rows affected (0.00 sec)

mysql> show tables;
+------------------------+
| Tables_in_kangbazi1805 |
+------------------------+
| python05               |
+------------------------+
1 row in set (0.00 sec)
```



## mysql数据类型  字符集 索引  引擎  

### mysql数据类型  

* 数值类型（整型 浮点型 ）

* 字符类型 

* 日期类型 

* 复合类型 

* 空间类型  

  

#### 整型   

| 数据类型  | 所占字节数 | 范围         |
| --------- | ---------- | ------------ |
| tinyint   | 1          | -128~127     |
| smallint  | 2          | -32767~32768 |
| int       | 4          |              |
| bigint    | 8          |              |
| mediumint | 3          |              |

**存性别 使用无符号的tinyint 0 男 1女 2表示未知  没有负数 所以无符号即可**　　

**年龄也是无符号的整型**

**设计表的时候能用小的字段类型 就不用  大的 ** 

'123.5456'

####　浮点类型　

| 数据类型 | 所占字节数 | 范围                                                 |
| -------- | ---------- | ---------------------------------------------------- |
| float    | 4          | 单精度                                               |
| double   | 8          | 双精度                                               |
| decimal  |            | 字符串的浮点数   也叫做定点数 要求精度比较高的使用它 |



#### 字符类型  

| 数据类型   | 所占字节数 | 范围                              |
| ---------- | ---------- | --------------------------------- |
| char       | 0-255      | 定长字符串                        |
| varchar    | 0-255      | 变长                              |
| tinytext   | 0-255      | 短文本字符串                      |
| text       |            | 长文本                            |
| blob       |            | 二进制形式的长文本数据 图像和音频 |
| longblob   |            | 极大二进制文本数据                |
| mediumblob |            | 中等二进制文本数据                |
| mediumtext |            | 中等文本数据                      |
| tinyblob   |            | 微小二进制文本数据                |
| longtext   |            | 长文本                            |
|            |            |                                   |
|            |            |                                   |



> 定长和变长  char 和varchar  
>
> char（64） 超过超过这个范围的值 被截短 不够的用空格补全 
>
> varchar(64） 不够的不会用空格补 超过的还是被截短 
>
> varchar 能够节约空间 提升空间利用率 



> blob 用来存放大型数据中的文本 图像 音频文件 
>
> text  和  blob 区别  
>
> text 不区分大小写  
>
> blob 严格区分大小写   



#### 时间类型   

| 类型      | 所占字节 | 范围                                |
| --------- | -------- | ----------------------------------- |
| date      | 3        | 2018-07-30                          |
| time      | 3        | 11:22:55                            |
| datetime  | 8        | 2018-07-30 11:22:55                 |
| timestamp | 4        | 自动记录你的修改时间 不用用户写进来 |
| year      | 1        | 返回当前时间的年份  2018            |

#### 复合类型 

| 类型 | 类型     | 范围                      |
| ---- | -------- | ------------------------- |
| set  | 集合类型 | set("num1","num2","num3") |
| enum | 枚举类型 | enum("num1","num2")       |

> 一个enum 只允许从集合中取一个   
>
> set 可以从集合中取多个  



#### 其它补充  

unsigned  表示  只能是正整数  

zerofill   

举例子: classnum int(11) zerofill   表示对班级号自动填充0   只能是 整型或者浮点型   

default 

​	    personnum  int(11) default '值'   只能是整型或者浮点型   默认值   表示字段的默认值

not null

​	  不能为空  放到后面  



### 字符集  

* gbk     前身叫做 gb2312 汉字 
* ASCII   美国标准信息交换代码   1
* Unicode  万国码  
* utf8    万国码识别的可变长度的字符编码    可以理解为 通用的转化格式    

### 工作中使用到的  

| 编码             | 说明                    |
| ---------------- | ----------------------- |
| gbk_chinsese_ci  | 简体中文   不区分大小写 |
| utf8_gerneral_ci | 多语言  不区分大小写    |



### 索引   好比字典的目录     为了快速查询 到对应的数据    

* 普通索引  任何字段都可以添加索引   
*  唯一索引    要求这一列不能有重复值 
* 主键索引    特殊的唯一索引 不允许有空值  不允许有空值 
* 全文索引     对全局数据进行查找    





1. create index   不能创建 主键索引
2. alter table    可以创建主键索引  

####　普通索引

``` 
? index  
				   索引的名字  表名(字段名)
mysql> create index in_name on python05(username);
Query OK, 2 rows affected (0.03 sec)
	   alter table python05 add index(username);
查看索引  
	   show index from 表名\G 
删除索引		          索引的名字  表名 
	   mysql> drop index in_name on python05; 

```



#### 唯一索引 这一列不能有重复值   

```
mysql> create unique index un_names on python05(username);    有重复值 创建失败  
Query OK, 2 rows affected (0.03 sec) 
	  alter table python05 add unique(username);
drop index un_names on python05;
```

#### 主键索引  删除主键索引的时候 首先 消除自增    

```
mysql> alter table python05 modify id int(11) unsigned not null;  消除自增  
mysql> alter table python05 drop primary key;  删除主键

mysql> alter table python05 add primary key(id);添加主键   
	   alter table python05 modify id int(11) unsigned auto_increment not null; 添加自增   
	  
```

#### 全文索引  

```
mysql> create fulltext index full_text on python05(username);
	   alter table python05 add fulltext(username);
```

#### 复合索引 

同时给两个字段添加索引  

```
alter table python05 add fulltext(username,content);
```



## 增删改查   

### 增  insert  

```
1.insert into 表名 values(); 这个必须严格按照字段名 及顺序  对应写 
2. insert into 表名(字段1,字段2) values(值1，值2); 有些字段加默认值 我们在插入数据的过程中可以忽略这些 只要按照 字段1 字段2。。。对应来写 

mysql> insert into python05 values('haha',1,1,10,112);  插入的过程中注意数据类型  要求整型  不能写入字符串 
Query OK, 1 row affected (0.00 sec)

mysql> insert into python05(username,sex,age) values('haha',1,30); 这里 id 自增  修改时间 自动 我们可以不用手动插入   
Query OK, 1 row affected (0.00 sec)

一次性 插入多条记录  
 insert into python05(username,sex,age) values('a',1,30),('b',0,20),('c',1,25);
```

##### 查  select 

```
mysql> create table money(
	id int(11) unsigned primary key auto_increment not null,
	username varchar(50) not null 
	balance float not null, 余额
	province varchar(20) not null, 省份
	age tinyint unsigned default '18', 年龄
	sex tinyint not null 性别
	)engine=innodb default charset=utf8;

 select * from 表名；
 * 展示所有的字段   
 
 mysql> select username,province from money; 查询指定的字段  
+----------+----------+
| username | province |
+----------+----------+
| 黄晓明         | 山东        |
| 杨颖        | 上海        |
| 范冰冰         | 山东        |
| 薛之谦      | 上海        |
| 周立波         | 上海        |
| 王思聪        | 北京         |
| 杨幂         | 浙江       |
+----------+----------+
7 rows in set (0.00 sec)

 查找到指定的字段 并且按照自定义的别名显示  
 select username as '用户名',province as '省' from money;
+--------+------+
| 用户名      | 省    |
+--------+------+
| 黄晓明       | 山东    |
| 杨颖      | 上海    |
| 范冰冰       | 山东    |
| 薛之谦    | 上海    |
| 周立波       | 上海    |
| 王思聪      | 北京     |
| 杨幂       | 浙江   |
+--------+------+
```

#### 更改乱码方式  

```
show variables like "%char%";
+--------------------------+---------------------------------------------------------+
| Variable_name            | Value                                                   |
+--------------------------+---------------------------------------------------------+
| character_set_client     | utf8                                                    |
| character_set_connection | utf8                                                    |
| character_set_database   | utf8                                                    |
| character_set_filesystem | binary                                                  |
| character_set_results    | utf8                                                    |
| character_set_server     | utf8                                                    |
| character_set_system     | utf8                                                    |
| character_sets_dir       | C:\Program Files\MySQL\MySQL Server 5.7\share\charsets\ |
+--------------------------+---------------------------------------------------------+
8 rows in set, 1 warning (0.00 sec)

set character_set_database=gb2312
set character_set_results=gb2312
set character_set_server=gb2312
set character_set_client=gb2312
```

### 查询单个字段 不重复 记录 

> select distinct 字段 from  表 

```
mysql> select distinct age from money;
+------+
| age  |
+------+
|   18 |
|   30 |
|   36 |
|   49 |
|   24 |
+------+
```

###　条件　　ｗｈｅｒｅ　　

```
 select * from money where id>2 and id<7;
 
 select * from money where id>1 and province="北京";
```

| 运算符 | 说明 |
| ------ | ---- |
| >      |      |
| <      |      |
| =      |      |
| >=     |      |
| <=     |      |
| !=     |      |
| and    | 并且 |
| or     | 或者 |
|        |      |



### 对结果集进行排序  　

```
select * from money ORDER BY balance DESC;   取出所有的结果并且 对结果进行排序 
select * from money ORDER BY balance ASC;
```

* desc  降序  
* asc  升序   

### 多字段进行排序 order by  

```
select * from money ORDER BY balance desc,age asc; 
如果 balance 已经排序 成功  那么第二个age 会被忽略 
如果第一个排序不成功 那么按照第二个 进行排序 
```

### 限制结果集  limit   

```
select * from money ORDER BY balance desc,age asc LIMIT 3; 当结果按照条件查询来 只取结果的前3条记录  

```

### 分页原理  

```
select * from money ORDER BY balance desc,age asc LIMIT 偏移量，数量；
123 是用户传给服务器    3条记录 写死的  只需要 计算   偏移量就可以  
第一页 limit 0,3；从第0个位置开始取3条记录    1 0 3
第二页 limit 3,3；从第3个位置开始取3条记录    2 3 3
第三页 limit 6,3  从第6个位置开始取3条记录    3 6 3 
```



### 统计函数   

* 班级总人数 
* 表里到底谁最有钱
* 明星的平均薪资 
* 明星的总金额 

| 函数    | 说明   |
| ------- | ------ |
| sum()   | 求和   |
| avg()   | 平均数 |
| count() | 总数   |
| max()   | 最大值 |
| min()   | 最小值 |
| between | 之间   |

#### 查询最大值  

```
select max(balance) from money;
```

#### 查询最小值

```
select min(balance) from money;
```

#### 统计总数

```
 select count(*) from money;
```

####　求和 

```
select sum(balance) from money; 
```

#### 平均数 

```
 select avg(age) from money;
```

#### 查询范围  

```
select * from money where age between 20 and 30; 
查询年龄在20岁到30岁之间的   
```



#### 分组  group by   一个省份一个组  重点   

```
select * from money GROUP BY province;

2	杨颖	456.67	上海	30	1
6	王思聪	44543400	北京	24	0
1	黄晓明	123.456	山东	18	0
7	杨幂	3345560	浙江	24	1


select COUNT(province),province from money  GROUP BY province; 查看组成员数量 

3	上海
1	北京
2	山东
1	浙江

```



#### with rollup 在分组统计的基础上  也就是在上面基础上 进行一次总数的统计   了解   

```
3	上海
1	北京
2	山东
1	浙江
7	       比较上面 多了一行 总数 

```



####　统计结果再过滤　having　　分组统计之后 查询 组成员大于1个的  记录

```
select COUNT(province) as result,province from money GROUP BY province HAVING result>1;

3	上海
2	山东

```

####　整合　

语法结构　　

select 

[字段1 [as 别名],[函数(字段2)]，字段n。。。。。。]　  选择的列  

from  表名    

[where 条件]   查询的条件  

[group by 字段 ]   分组

[having 条件]  过滤条件  

[order by] 排序 

[limit 偏移量，数量]





```
mysql> select id,username,balance,province from money where id>1 and balance>125 group by province order by id desc limit 3;
```





### 多表联合查询  

```
准备两张表   
1.user表  
	mysql> create table user(uid int(11) not null,username varchar(30) not null,password char(32) not null,primary key(uid))engine=innodb default charset=utf8;

2.订单表
    create table order_goods(oid int(11) unsigned primary key auto_increment not null,uid int(11) not null,name varchar(64) not null,buytime int(11) not null )engine=innodb default charset=utf8;
      
```

* 内连接  

  * inner join on

  ```
  mysql> select 
  	user.username as 'yonghuming',
  	order_goods.name as 'shangpinming',
  	order_goods.buytime 
  	from user inner join order_goods
      on user.uid=order_goods.uid;
  +------------+--------------+----------+
  | yonghuming | shangpinming | buytime  |
  +------------+--------------+----------+
  | 景甜           | mac pro      |     1234 |
  | 岳云鹏           | 小杜            |     1213 |
  | 高峰          | 娃娃             |   123223 |
  | 千锋          | 鼠标             | 12345566 |
  | 郭德纲          | 法拉利             |    12221 |
  +------------+--------------+----------+
  5 rows in set (0.00 sec)
  ```

  

  *  给表名 起别名 不用加as  直接空格 就好  给字段添加别名  用as 

  ```
  mysql> select 
  	u.username as '用户名',
  	o.name as '商品名',
  	o.buytime 
  	from 
  	user u,
  	order_goods o 
  	where u.uid=o.uid;
  +--------+---------+----------+
  | 用户名      | 商品名       | buytime  |
  +--------+---------+----------+
  | 景甜       | mac pro |     1234 |
  | 岳云鹏       | 小杜       |     1213 |
  | 高峰      | 娃娃        |   123223 |
  | 千锋      | 鼠标        | 12345566 |
  | 郭德纲      | 法拉利        |    12221 |
  +--------+---------+----------+
  5 rows in set (0.01 sec)
  ```

  

* 外连接 

  * 左连接 left join on  左连接 以left 左边的表为准  * 查询所有的记录  所有买过的没买的全查出来 因为以用户表为准

  ```
  mysql> select * from user left join order_goods on user.uid=order_goods.uid;
  +-----+----------+-----------+------+------+---------+----------+
  | uid | username | password  | oid  | uid  | name    | buytime  |
  +-----+----------+-----------+------+------+---------+----------+
  |   1 | 景甜         | 123321    |    1 |    1 | mac pro |     1234 |
  |   3 | 岳云鹏         | 123qwe    |    2 |    3 | 小杜       |     1213 |
  |   9 | 高峰        | 123311    |    3 |    9 | 娃娃        |   123223 |
  |   8 | 千锋        | 123qwq    |    4 |    8 | 鼠标        | 12345566 |
  |   5 | 郭德纲        | 1234aafsa |    5 |    5 | 法拉利        |    12221 |
  |   2 | 张继科       | 123456    | NULL | NULL | NULL    |     NULL |
  |   4 | 沈腾         | 123qwqe   | NULL | NULL | NULL    |     NULL |
  |   6 | 玛丽         | 123sdwsa  | NULL | NULL | NULL    |     NULL |
  |   7 | 马什么梅      | 123aesa   | NULL | NULL | NULL    |     NULL |
  +-----+----------+-----------+------+------+---------+----------+
  9 rows in set (0.01 sec)
  ```

  

  * 右连接 right join on   以right join 右边的表为准 只查出来谁买了什么东西 没买的就不显示 

  ```
  mysql> select * from user right join order_goods on user.uid=order_goods.uid;
  +------+----------+-----------+-----+-----+---------+----------+
  | uid  | username | password  | oid | uid | name    | buytime  |
  +------+----------+-----------+-----+-----+---------+----------+
  |    1 | 景甜         | 123321    |   1 |   1 | mac pro |     1234 |
  |    3 | 岳云鹏         | 123qwe    |   2 |   3 | 小杜       |     1213 |
  |    9 | 高峰        | 123311    |   3 |   9 | 娃娃        |   123223 |
  |    8 | 千锋        | 123qwq    |   4 |   8 | 鼠标        | 12345566 |
  |    5 | 郭德纲        | 1234aafsa |   5 |   5 | 法拉利        |    12221 |
  +------+----------+-----------+-----+-----+---------+----------+
  5 rows in set (0.00 sec)
  ```

  

* 子查询

  * in（） 先从订单表中查询出来所有的 uid   子 

    ​	   在从用户表中查询这些uid 对应的所有信息  父 

  ```
  mysql> select * from user where uid in(select uid from order_goods);
  +-----+----------+-----------+
  | uid | username | password  |
  +-----+----------+-----------+
  |   1 | 景甜         | 123321    |
  |   3 | 岳云鹏         | 123qwe    |
  |   9 | 高峰        | 123311    |
  |   8 | 千锋        | 123qwq    |
  |   5 | 郭德纲        | 1234aafsa |
  +-----+----------+-----------+
  5 rows in set (0.01 sec)
  ```

  