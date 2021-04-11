### 启动mysql

安装好mysql后，先启动

```bash
sudo systemctl start mysqld		#注意mysqld，不是mysql
```

设置当系统启动时自动启动

```bash
sudo systemctl enable mysqld
```

查看其启动状态

```bash
sudo systemctl status mysqld
```

### 修改密码策略

安装好mysql后，已经自动为root账户设置了临时密码，要把临时密码更换后才能进行操作。

```bash
cat /var/log/mysqld.log | grep -i 'temporary password'	#打印临时密码
```

```bash
2020-03-13T04:42:05.613193Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: =27sGUV-tNh4	#最后的冒号后面的字符串就是临时密码，不包括空格
```

因为高版本的mysql默认无法使用简单密码，会报错`ERROR 1819 (HY000): Your password does not satisfy the current policy requirements……`，要更改密码策略。

先进入mysql的命令行界面：

```bash
sudo mysql -u root -p
Enter password:	   <--输入临时密码
```

查看密码策略

```mysql
mysql> SHOW VARIABLES LIKE 'validate_password%';
```

修改密码策略

```mysql
#8.0
mysql> set global validate_password.policy=0;	#密码强度检查等级设置为0(low)

#5.7
mysql> set global validate_password_policy = 0;

Query OK, 0 rows affected (0.00 sec)	#有此提示才表示执行成功
```

| 密码强度Policy | 密码强度Policy | 检查形式Tests Performed                                  |
| -------------- | -------------- | -------------------------------------------------------- |
| 0              | LOW            | 符合长度                                                 |
| 1（默认）      | MEDIUM         | 符合长度；有数字，字母，符号                             |
| 2              | STRONG         | 符合长度；有数字，字母，符号；验证密码强度的字典文件路径 |

```mysql
#8.0
mysql> set global validate_password.length=4;	#密码最小长度设置为4，不能小于4

#5.7
mysql> set global validate_password_length=4;

Query OK, 0 rows affected (0.00 sec)
```

设置成功，退出mysql界面

```mysql
mysql> quit
```

### 修改密码

在bash界面，使用mysql安全向导配置

```ba
mysql_secure_installation
```

开始设置

```bash
Enter password for user root:	<--输入临时密码

New password:			<--输入新密码
Re-enter new password:	<--确认新密码

Do you wish to continue with the password provided?		<--输入y|Y确认修改

Remove anonymous users?		<--移除匿名账户？Y|y是，其他任意键否

Disallow root login remotely?	<--禁止root远程登录？Y|y禁止，其他键不禁止

Remove test database and access to it?	<--删除test数据库？

Reload privilege tables now? 	<--重新加载权限表？
```

以上需要选择的配置，输入Y或者y表示是，其他任意键表示否。

可以用新密码登录root了

```bash
mysql -u root -p
Enter password:		<--输入新密码
```

### 修改编码

打开mysql配置文件

```bash
sudo vim /etc/my.cnf
```

加入以下信息

```txt
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

重启myql

```bash
sudo systemctl restart mysqld
```

### 开放root账户

为了能在远端连接mysql

先进入mysql界面

```bash
mysql -u root -p
```

```mysql
mysql> use mysql
Database changed
```

给root用户设置为所有ip都可访问：

```
update user set host='%' where user='root' and host='localhost';
```

列出用户名及其可访问的地址

```my
mysql> select user, host from user;
```

```mysql
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| root             | %         |
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
+------------------+-----------+
4 rows in set (0.00 sec)
```

刷新权限

```mysql
    mysql> flush privileges;
```

重启

```bash
sudo systemctl restart mysqld
```

### 操作

#### 1. 新建账户

```mysql
mysql> CREATE USER '用户名'[@'可登录主机'] [IDENTIFIED BY '密码'];	
#[]为可选内容，若只想在本地登录，用localhost，若想在任意主机登录，用%
#密码可为空，则不需密码也可登录
```

#### 2. 账户授权

```sql
GRANT 权限 ON 数据库名.数据表名 TO '用户名'@'可操作主机'
```

- 权限为`ALL`时指所有权限

- 若要对一个数据库内的所有表都授权，可用`database.*`

- 若对所有数据库的所有表授权，`数据库名.数据表名` 可以用`*`代替

- 主机可为`%`，表示任意主机

例如

```mysql
grant all on user.* to 'root'@'%';
flush privileges;	-- 刷新权限
```

### Navicat连接问题

用Navicat连接Mysql 8版本时会出现错误，错误码`2059`。新版的mysql使用`caching_sha2_password`验证方式，而navicat使用`mysql_native_password`，所以出现错误。

在mysql命令行中执行命令

```mysql
mysql> ALTER USER '用户名'@'登录主机' IDENTIFIED WITH mysql_native_password BY '验证密码';
```



---

> 参考：
>
> https://help.aliyun.com/document_detail/116727.html
>
> https://juejin.im/post/5c088b066fb9a049d4419985
>
> https://www.cnblogs.com/ivictor/p/5142809.html
>
> https://blog.csdn.net/kuluzs/article/details/51924374
>
> https://blog.csdn.net/vsiryxm/article/details/44220551



