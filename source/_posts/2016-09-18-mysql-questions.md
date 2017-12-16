---
layout: post
title:  "MySQL 备忘"
date:   2016-09-17
excerpt: "记录MySQL相关的一些问题，供参考"
tags: 
  - database
comments: true
---

> 记录MySQL相关的一些问题，供参考

### 自动导出的SQL脚本中，有一些注释``/*!40000 DROP DATABASE IF EXISTS `demo`*/`` 是什么意思

Comments of the form /*! stuff */ are treated as comments by other RDBMSs, but MySQL will read what's inside the comment and execute it as SQL. You can use this to take advantage of MySQL-specific features even using code that might be run against other RDBMSs. For example you could use /*! ENGINE=INNODB */ in a CREATE TABLE query.

The numbers are optional and if you use them then MySQL will ignore them if its version number is less than the number (with dots inserted in the appropriate places).

更详细内容，参考[mysql 手册].

*** 

### MySQL在Mac OS X下升级

略麻烦，参考官方给的[Logical Upgrade](http://dev.mysql.com/doc/refman/5.7/en/upgrading.html#upgrade-procedure-logical)方法。

1. 先备份所有数据

```
mysqldump -u root -p
  --add-drop-table --routines --events
  --all-databases --force > data-for-upgrade.sql
```

2. 关闭数据库

```
mysqladmin -u root -p shutdown
```

3. 下载DMG的安装文件，安装新版数据库

- 注意安装完成时，界面提示的随机初始密码，记下来！
- 这种安装方式，默认已经进行了数据目录的初始化，不需要额外进行。

4. 启动数据库

```
mysqld_safe --user=mysql
```

5. 重置新数据库密码

```
shell> mysql -u root -p
Enter password: ****  <- enter temporary root password
mysql> ALTER USER USER() IDENTIFIED BY 'your new password';
```

6. 导入旧数据库中数据

```
mysql -u root -p --force < data-for-upgrade.sql
```

7. 运行升级程序

```
mysql_upgrade -u root -p
```

两个作用：更新不兼容数据; 更新系统数据库;

8. 配置slow shutdown, 然后重启数据库

```
mysql -u root -p --execute="SET GLOBAL innodb_fast_shutdown=0"
mysqladmin -u root -p shutdown
mysqld_safe --user=mysql (直接进入设置重启更方便)
```

[mysql 手册]: http://dev.mysql.com/doc/refman/5.7/en/comments.html