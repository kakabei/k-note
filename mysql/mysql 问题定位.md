
问题1 

 Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)

```
这个问题可能是由于mysql的客户端没能找到mysql.sock这个文件。socket的两种连接方式：一种是TCP，一是本UNIX文件。
所以可以查看/etc/mysql/my.conf配置文件

[mysqld]
socket          = /var/run/mysqld/mysqld.sock
[client]
port            = 3306
#socket          = /var/run/mysqld/mysqld.sock
socket          =/tmp/mysql.sock 

这两个地方的socket不一样，改成一样的就好。


另一种可能是服务没有起。
```
