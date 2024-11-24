
**ubuntu 安装**

```bash
sudo apt update
sudo apt install mysql-server

```
查看状态
```
sudo service mysql status
```

查看所有链接到mysql 了情况：

```
netstat -tap | grep mysql
```

mysql 安全性设置: [https://ui-code.com/archives/627](https://ui-code.com/archives/627)

创建用户及用户权限： [https://www.cnblogs.com/zhongyehai/p/10695659.html](https://www.cnblogs.com/zhongyehai/p/10695659.html)


创建用户

```shell 
CREATE USER 'dev_root'@'%' IDENTIFIED BY 'king+test';
GRANT privileges ON databasename.tablename TO 'dev_root'@'%'; 
```

修改新密码
```
use mysql;
update user set password=PASSWORD('新密码') where user='用户名';
flush privileges; #更新权限
quit; #退出

```

增加新用户

格式:grant select on 数据库.* to 用户名@登录主机 identified by '密码'
举例: 增加一个用户 test1 密码为 abc,让他可以在任何主机上登录,并对所有数据库有
查询、插入、修改、删除的权限。首先用以 root 用户连入 MySQL,然后键入以下命令:
```
mysql>grant select,insert,update,delete on *.* to root@localhost identified by 'mysql';
# 或者
grant all privileges on *.* to root@localhost identified by 'mysql';
然后刷新权限设置。
flush privileges;
```

如果你不想 root 有密码操作数据库 "mydb" 里的数据表,可以再打一个命令将密码消掉。
```
grant select,insert,update,delete on mydb.* to root@localhost identified by '';
```

删除用户
```
mysql>delete from user where user='用户名' and host='localhost';
mysql>flush privileges;
**mysql win10 安装方法**
```

[Windows 10 安装 MySQL 8.0 指南 : https://zhuanlan.zhihu.com/p/75891315](https://zhuanlan.zhihu.com/p/75891315)


1 下载：[https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.23-winx64.zip](https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.23-winx64.zip)

2 配置文件
```mysql 

[mysql]

# 设置mysql客户端默认字符集
default-character-set=utf8

[mysqld]

# 设置3306端口
port = 3306

# 设置mysql的安装目录
basedir=C:\Program Files\MySQL

# 设置mysql数据库的数据的存放目录
datadir=C:\Program Files\MySQL\data

# 允许最大连接数
max_connections=20

# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8 

# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

```
  
```sh
.\mysqld.exe install

mysqld --install MySQL 
.\mysqld.exe --initialize --console
```

A temporary password is generated for root@localhost: cj:hbzhDX8aN

更改密码： 

``` sql
ALTER USER 'root'@'localhost' identified with mysql_native_password by 'king+1234'
CREATE USER 'root'@'%' IDENTIFIED BY 'king+5688';

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
FLUSH PRIVILEGES;
```



启动服务 ： net start mysql
关闭服务： net stop mysql
连接 mysql mysql -u root -p



# 一、数据库

```shell
# 登录

mysql -h 127.0.0.1 -u <用户名> -p<密码>.   ##  默认用户名<root>，-p 是密码，参数后面不需要空格

mysql -D 所选择的数据库名 -h 主机名 -u 用户名 -p

mysql> exit  ## 退出 

mysql> status;   ## 显示当前mysql的version的各种信息

mysql> select version();  ## 显示当前mysql的version信息

mysql> show global variables like 'port';  ## 查看MySQL端口号
```



```mysql 
--  创建一个数据库
create database database_name character set gbk; 

-- 删除数据库
drop database database_name;                     

-- 显示数据库列表
show databases;                                  

-- 切换数据库
use database_name;                                  
```

# 二、表


```mysql 
-- 表操作

-- 显示 samp_db 下面所有的表名字
show tables;                 

-- 显示数据表的结构
describe table_name;         

-- 清空表中记录
delete from table_name;       

-- 用于删除数据库中的现有表。
drop table table_name;        

-- 用于删除表内的数据，但不删除表本身。
truncate table table_name;   

-- 修改表名
alter table table_name rename new_table_name;


```

**表创建**

```mysql 
CREATE TABLE `UserInfo` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '编号ID',
  `name` varchar(64) NOT NULL DEFAULT '' COMMENT '用户名',
  `create_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
  `update_time` datetime NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '更新时间',
  `extendInfo` varchar(1024) NOT NULL DEFAULT '' COMMENT '扩展字段',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

表的一些属性说明：

- `NULL`：数据列可包含 NULL 值；
- `NOT NULL`：数据列不允许包含 NULL 值；
- `DEFAULT`：默认值；
- `PRIMARY KEY`：主键；
- `AUTO_INCREMENT`：自动递增，适用于整数类型；
- `UNSIGNED`：是指数值类型只能为正数；
- `CHARACTER SET name`：指定一个字符集；
- `COMMENT`：对表或者字段说明；


**一些操作**

```mysql 
-- 添加列
alter table 表名 add 列名 列数据类型 [after 插入位置];

-- 在 age 后添加 birthday 
alter table Userinfo add birthday varchar(100) not null default "" date after age;

-- 修改列
alter table 表名 change 列名称 列新名称 新数据类型;

-- 将 name 列的数据类型改为 char(16): 
alter table Userinfo change name name char(16) not null;

-- 更新字段长度
alter table address modify column city char(30);

-- 删除列
alter table 表名 drop 列名称;

-- 删除 birthday 这一列
alter table Userinfo drop birthday;

```

# 三、常用语句

```mysql

-- SELECT
select id, name from UserInfo where id = 111; 

-- update 语句 根据条件更新对应的字段
update UserInfo set name='南山肯德鸡'  where id = 111; 

-- 插入 INSERT
insert into UserInfo(name, create_time，update_time) value ('夏有凉风'， '2020-11-10 08:33:21', '2020-11-10 08:33:21');

-- 从一个表的数据插入到另外一个表
insert into live (user_id, name) SELECT m.user_id, m.name FROM moot m where m.id=111;

-- 如不存在则插入，如存在则更新用 : INSERT INTO ON ... DUPLICATE KEY UPDATE
INSERT INTO `charger` (`id`,`type`,`create_at`,`update_at`) VALUES (3,2,'2017-05-18 11:06:17','2017-05-18 11:06:17') ON DUPLICATE KEY UPDATE `id`=VALUES(`id`), `type`=VALUES(`type`), `update_at`=VALUES(`update_at`);

--  DELETE 现实开发实践中，业务上一般不允许删除。DBA 要把这个权限收回。

-- 清空表
delete from  UserInfo
-- 或
delete * from  UserInfo

-- 删除指定的一行
delete from  UserInfo  WHERE id = 111; 

--  ORDER BY用于按升序或降序对结果集进行排序。 默认按 asc 。  asc 升序排序, desc 降序排序。 
-- 升序
select * from UserInfo order by  id asc;  
-- 降序
select * from UserInfo order by id desc;

-- GROUP BY 将具有相同值的行分组到汇总。

-- 分组汇总不同性别的人数
select male, count(id) from UserInfo group by male; 

-- UNION 合并两个或多个 SELECT 语句的结果集。
select name from Employees_ZH union select name from Employees_USA; 

```



 **JOIN**

JOIN 子句用于根据两个或多个表之间的相关列组合来自两个或多个表的行。

- `INNER JOIN` : 两个表都存在的行，才返回。
- `LEFT JOIN`:  右表没有，左表有，也要返回。
- `RIGHT JOIN`: 左表没有，右表有，也要返回。
- `FULL JOIN`: 只要其中一个表中有，就返回行。 (MySQL 是不支持的，通过 `LEFT JOIN + UNION + RIGHT JOIN` 的方式 来实现)

 **FULL OUTER JOIN**

当左（表1）或右（表2）表记录中存在匹配时，关键字返回所有记录

# 四、SQL函数



```mysql 
AVG--  COUNT 统计满足条件的行数。
select male, count(id) from UserInfo group by male; 

-- AVG 数值列的平均值。
SELECT AVG(列名称) FROM 表名称 WHERE 条件;

-- SUM  数值列的总和
SELECT SUM(列名称) FROM 表名称 WHERE 条件;

-- MAX 所选列的最大值
SELECT MIN(列名称) FROM 表名称 WHERE 条件;

-- MIN 所选列的最小值
SELECT MIN(列名称) FROM 表名称 WHERE 条件;

-- 
```

# 五、索引

```mysql 
--  添加索引 普通索引(INDEX)
ALTER TABLE `表名字` ADD INDEX 索引名字 ( `字段名字` )

-- 直接创建索引
CREATE INDEX index_user ON Userinfo(name)

-- 修改表结构的方式添加索引
ALTER TABLE table_name ADD INDEX index_name ON (column(length))

-- 给 表中的 name 字段 添加普通索引
ALTER TABLE `Userinfo` ADD INDEX index_name (name)

-- 创建表的时候同时创建索引
CREATE TABLE `user` (
    `id` int(11) NOT NULL AUTO_INCREMENT ,
    `title` char(255) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL ,
    `content` text CHARACTER SET utf8 COLLATE utf8_general_ci NULL ,
    `time` int(10) NULL DEFAULT NULL ,
    PRIMARY KEY (`id`),
    INDEX index_name (title(length))
)

-- 删除索引
DROP INDEX index_name ON table


-- 主键索引
ALTER TABLE `表名字` ADD PRIMARY KEY ( `字段名字` )

-- 唯一索引
ALTER TABLE `表名字` ADD UNIQUE (`字段名字`)

-- 全文索引
ALTER TABLE `表名字` ADD FULLTEXT (`字段名字`)

--  添加多列索引
ALTER TABLE `table_name` ADD INDEX index_name ( `column1`, `column2`, `column3`)

```

#  导入导出

从数据库导出数据库文件 使用 mysqldump 命令

导出所有数据库

```
mysqldump -u [数据库用户名] -p -A>[备份文件的保存路径]
```

导出数据和数据结构
```
mysqldump -u [数据库用户名] -p [要备份的数据库名称]>[备份文件的保存路径]

 mysqldump -h localhost -u root -p mydb >e:\MySQL\mydb.sql
```


将数据库 mydb 中的 mytable 导出到 e:\MySQL\mytable.sql 文件中。
```
c:\> mysqldump -h localhost -u root -p mydb mytable>e:\MySQL\mytable.sql
```

将数据库 mydb 的结构导出到 e:\MySQL\mydb_stru.sql 文件中。

```
c:\> mysqldump -h localhost -u root -p mydb --add-drop-table >e:\MySQL\mydb_stru.sql
```

只导出数据不导出数据结构

```
mysqldump -u [数据库用户名] -p -t [要备份的数据库名称]>[备份文件的保存路径]

```
导出数据库中的Events

```
mysqldump -u [数据库用户名] -p -E [数据库用户名]>[备份文件的保存路径]
```

导出数据库中的存储过程和函数

```
mysqldump -u [数据库用户名] -p -R [数据库用户名]>[备份文件的保存路径]
```

从外部文件导入数据库中

使用 source命令

```
mysql>source [备份文件的保存路径]
```

使用`<`符号
```
mysql -u root –p < [备份文件的保存路径]
```
# processlist 命令

processlist 命令的输出结果显示了有哪些线程在运行，可以帮助识别出有问题的查询语句，两种方式使用这个命令。

进入mysql/bin目录下输入mysqladmin processlist;

启动mysql，输入show processlist;

如果有SUPER权限，则可以看到全部的线程，否则，只能看到自己发起的线程（这是指，当前对应的MySQL帐户运行的线程）。

得到数据形式如下（只截取了三条）：

```sh
mysql> show processlist;

+-----+-------------+--------------------+-------+---------+-------+----------------------------------+----------

| Id | User | Host           | db  | Command | Time| State    | Info                                                                                          

+-----+-------------+--------------------+-------+---------+-------+----------------------------------+----------

|207|root |192.168.0.20:51718 |mytest | Sleep   | 5   |        | NULL                                                                                                

|208|root |192.168.0.20:51719 |mytest | Sleep   | 5   |        | NULL        

|220|root |192.168.0.20:51731 |mytest |Query   | 84  | Locked |


```

先简单说一下各列的含义和用途:

- id ：不用说了吧，一个标识，你要kill一个语句的时候很有用。
- user ：显示单前用户，如果不是root，这个命令就只显示你权限范围内的sql语句。
- host ：显示这个语句是从哪个ip的哪个端口上发出的。呵呵，可以用来追踪出问题语句的用户。
- db ：显示这个进程目前连接的是哪个数据库。
- command：显示当前连接的执行的命令，一般就是休眠（sleep），查询（query），连接（connect）。
- time：此这个状态持续的时间，单位是秒。
- state：显示使用当前连接的sql语句的状态，很重要的列，后续会有所有的状态的描述，请注意，state只是语句执行中的某一个状态，一个sql语句，已查询为例，可能需要经过copying to tmp table，Sorting result，Sending data等状态才可以完成，
- info: 显示这个sql语句，因为长度有限，所以长的sql语句就显示不全，但是一个判断问题语句的重要依据。

这个命令中最关键的就是state列，mysql列出的状态主要有以下几种：

  Checking table 正在检查数据表（这是自动的）。
　Closing tables 正在将表中修改的数据刷新到磁盘中，同时正在关闭已经用完的表。这是一个很快的操作，如果不是这样的话，就应该确认磁盘空间是否已经满了或者磁盘是否正处于重负中。
　Connect Out 复制从服务器正在连接主服务器。
　Copying to tmp table on disk 由于临时结果集大于tmp_table_size，正在将临时表从内存存储转为磁盘存储以此节省内存。
　Creating tmp table 正在创建临时表以存放部分查询结果。
　deleting from main table 服务器正在执行多表删除中的第一部分，刚删除第一个表。
　deleting from reference tables 服务器正在执行多表删除中的第二部分，正在删除其他表的记录。
　Flushing tables 正在执行FLUSH TABLES，等待其他线程关闭数据表。
　Killed 发送了一个kill请求给某线程，那么这个线程将会检查kill标志位，同时会放弃下一个kill请求。MySQL会在每次的主循环中检查kill标志位，不过有些情况下该线程可能会过一小段才能死掉。如果该线程程被其他线程锁住了，那么kill请求会在锁释放时马上生效。
　Locked 被其他查询锁住了。
　Sending data 正在处理Select查询的记录，同时正在把结果发送给客户端。
　Sorting for group 正在为GROUP BY做排序。
　Sorting for order 正在为ORDER BY做排序。
　Opening tables 这个过程应该会很快，除非受到其他因素的干扰。例如，在执Alter TABLE或LOCK TABLE语句行完以前，数据表无法被其他线程打开。正尝试打开一个表。
　Removing duplicates 正在执行一个Select DISTINCT方式的查询，但是MySQL无法在前一个阶段优化掉那些重复的记录。因此，MySQL需要再次去掉重复的记录，然后再把结果发送给客户端。 
　Reopen table获得了对一个表的锁，但是必须在表结构修改之后才能获得这个锁。已经释放锁，关闭数据表，正尝试重新打开数据表。
　Repair by sorting 修复指令正在排序以创建索引。
　Repair with keycache 修复指令正在利用索引缓存一个一个地创建新索引。它会比Repair by sorting慢些。
　Searching rows for update 正在讲符合条件的记录找出来以备更新。它必须在Update要修改相关的记录之前就完成了。
　Sleeping 正在等待客户端发送新请求.
　System lock 正在等待取得一个外部的系统锁。如果当前没有运行多个mysqld服务器同时请求同一个表，那么可以通过增加--skip-external-locking参数来禁止外部系统锁。
　Upgrading lock
　Insert DELAYED 正在尝试取得一个锁表以插入新记录。
　Updating 正在搜索匹配的记录，并且修改它们。
　User Lock正在等待GET_LOCK()。
　Waiting for tables该线程得到通知，数据表结构已经被修改了，需要重新打开数据表以取得新的结构。然后，为了能的重新打开数据表，必须等到所有其他线程关闭这个表。以下几种情况下会产生这个通知：FLUSH TABLES tbl_name, Alter TABLE, RENAME TABLE, REPAIR TABLE, ANALYZE TABLE,或OPTIMIZE TABLE。
　waiting for handler insert
　Insert DELAYED 已经处理完了所有待处理的插入操作，正在等待新的请求。
　
　大部分状态对应很快的操作，只要有一个线程保持同一个状态好几秒钟，那么可能是有问题发生了，需要检查一下。
　
　还有其他的状态没在上面中列出来，不过它们大部分只是在查看服务器是否有存在错误是才用得着。

mysql手册里有所有状态的说明，链接如下：http://dev.mysql.com/doc/refman/5.0/en/general-thread-states.html

# 六、其他

##  删除重复记录

```mysql 
-- 查找表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断
select * from people where peopleId in (select peopleId from people group by peopleId having count(peopleId) > 1)

-- 删除表中多余的重复记录，重复记录是根据单个字段（peopleId）来判断，只留有rowid最小的记录
delete from people 
where peopleId in (select peopleId from people group by peopleId having count(peopleId) > 1)
and rowid not in (select min(rowid) from people group by peopleId having count(peopleId )>1)

-- 查找表中多余的重复记录（多个字段）
select * from vitae a
where (a.peopleId,a.seq) in (select peopleId,seq from vitae group by peopleId,seq having count(*) > 1)

-- 删除表中多余的重复记录（多个字段），只留有rowid最小的记录
delete from vitae a
where (a.peopleId,a.seq) in (select peopleId,seq from vitae group by peopleId,seq having count(*) > 1) and rowid not in (select min(rowid) from vitae group by peopleId,seq having count(*)>1)

-- 查找表中多余的重复记录（多个字段），不包含rowid最小的记录
select * from vitae a
where (a.peopleId,a.seq) in (select peopleId,seq from vitae group by peopleId,seq having count(*) > 1) and rowid not in (select min(rowid) from vitae group by peopleId,seq having count(*)>1)

```

## 批量创建分表

```shell
for i in {0..1023}
do

mysql -h127.0.0.1 -uadmin -padmin123 -P3358 hgame  -e "CREATE TABLE UserInfo$i (

  id bigint(20) unsigned NOT NULL COMMENT 'SKU ID',
  createi_time varchar(255) NOT NULL DEFAULT '' COMMENT '导入时间',
  ext varchar(255) NOT NULL DEFAULT '',
  PRIMARY KEY (id)

) ENGINE=InnoDB DEFAULT CHARSET=utf8";

done
```


---

# 资料

1、[https://github.com/jaywcjlove/mysql-tutorial/blob/master/docs/21-minutes-MySQL-basic-entry.md](https://github.com/jaywcjlove/mysql-tutorial/blob/master/docs/21-minutes-MySQL-basic-entry.md)

2、[https://www.w3school.com.cn/sql/index.asp](https://www.w3school.com.cn/sql/index.asp)

3、mysql explain 命令详解 : [https://blog.xyecho.com/mysql-explain/](https://blog.xyecho.com/mysql-explain/)

4、mysql 表设计的21个经验准则 : [https://blog.xyecho.com/mysql-table-design-21-criterion/](https://blog.xyecho.com/mysql-table-design-21-criterion/)

5、mysql 索引知识点总结: [https://blog.xyecho.com/mysql-index/](https://blog.xyecho.com/mysql-index/)