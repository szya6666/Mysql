# Mysql 基础 

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