### INSERT

insert 用于增加记录。

#### 插入一条记录

```mysql
INSERT INTO 数据表名(字段名1, 字段名2, ...， 字段名n)
VALUES
	(值1, 值2, ..., 值n);
```

值的位置要和字段的位置相匹配，比如，数据值2不能放在第一个位置，不然就会被插入到字段1中去。向 user 表里插入数据，如果 `'管理员账户'` 放在第一个位置，那么 `UserID` 的值将会是 `管理员账户`，而非 `administrator`。

```mysql
INSERT INTO user(UserID, UserName, BirthDay)
VALUES
	('administrator', '管理员账户', NULL);
```

如若成功插入，则会提醒影响了几行

```
Query OK, 1 row affected (0.01 sec)
```

否则即插入失败。如果再执行一遍上述语句，就会因主键冲突而插入失败

```
ERROR 1062 (23000): Duplicate entry 'administrator' for key 'PRIMARY'
```

#### 插入多条记录

插入多条记录与插入1条记录类似，只要在 VALUES 后多写几条数据即可。

```mysql
INSERT INTO USER ( UserID, UserName, BirthDay )
VALUES
	( 'administrator1', '管理员账户1', NULL ),
	( '1', '1', '20080808' ),
	( '2', 'student', '19990101' );
```

#### 关于引号的使用

应使用单引号。除了数值型数据(如 int、integer 等)不需要使用引号外，其他任何类型的数据都应使用数据。虽然数值型数据也可以用引号括起来，但习惯上是不用的。

#### 从另一个表插入数据

```mysql
INSERT INTO 数据表名(字段名1, ..., 字段名n)
SELECT 字段名1, ..., 字段名n		-- 这是另一个表的字段名
FROM 另一个表名
[WHERE 限定条件];
```

如，把 user 表中所有性别为男的用户 id 和 姓名都插入到 user2 表中

```mysql
INSERT INTO user2(id, name)
SELECT UserID, UserName
FROM user
WHERE user.sex = 'M';
```

若两个表完全相同(字段个数及次序相同)，还可以使用 `SELECT *` 来插入数据

```mysql
INSERT INTO 数据表名
SELECT *
FROM 另一个表名;
```

若字段的个数不相同，则无法插入，提示列值数量不匹配

```
ERROR 1136 (21S01): Column count doesn't match value count at row 1
```

若字段个数相同，但是列与列之间的数据格式不兼容，也无法插入。

若字段个数和格式都相同，但列与列间的数据内容不匹配，则会出现下面的情况。第一个是 user 表，第二个是 user2 表。

![user表](https://i.loli.net/2020/04/10/yfXwG2ALKaN3oqM.png)

![user2表](https://i.loli.net/2020/04/10/vj4SME35DKacLdW.png)

### UPDATE

update 用于修改记录的值。

```mysql
UPDATE 表名 SET 字段名1 = 值1 WHERE 限定条件;
```

如，把 user 表中 ID 为 admin 的用户名改为 `超级用户`

```mysql
UPDATE USER 
	SET UserName = '超级用户' 
WHERE
	UserID = 'admin';
```

若要作用于所有记录，只要把 WHERE 子句去掉即可。如，把表中所有用户的生日设置为 NULL

```mysql
UPDATE USER 
	SET BirthDay = NULL;
```

若要修改一条记录的多个字段，可以在 SET 后多设置几个值，并用逗号隔开。如，把 user 表中 ID 为 admin 的用户名改为 `管理员`，并把Birthday 改为当前日期

```mysql
UPDATE user
	SET UserName = '管理员',
	BirthDay = NOW() 
WHERE
	UserID = 'admin';
```

### DELETE

DELETE 命令用于删除记录。

```mysql
DELETE FROM 表名 [WHERE 限定条件];
```

如果不带限定条件，那么整个表的所有记录全都会被删除，因此在使用 DELETE 命令时必须非常小心。

如，要删除所有性别为男的用户数据

```mysql
DELETE FROM user WHERE Sex = '男';
```

#### 注意

在没有 WHERE 子句时，要特别小心使用 UPDATE 和 DELETE 命令，如果没有使用 WHERE 子句设置条件，表里所有记录的相应字段都会被更新。在大多数情况下，DML 命令都需要使用 WHERE 子句。在使用 UPDATE 和 DELETE 命令之前，最好先用 SELECT 命令进行测试，确保要操作的数据就是自己想要的。