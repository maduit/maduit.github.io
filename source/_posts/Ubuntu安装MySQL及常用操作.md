---
title: Ubuntu安装MySQL及常用操作
tag: Spark
categories: 2023
---

[MySQL](http://www.mysql.com/)是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品。MySQL 最流行的关系型数据库管理系统，在 WEB 应用方面MySQL是最好的 RDBMS (Relational Database Management System，关系数据库管理系统) 应用软件之一。

## 一、安装MySQL

使用以下命令即可进行mysql安装，注意安装前先更新一下软件源以获得最新版本：

```
sudo apt-get update  #更新软件源
sudo apt-get install mysql-server  #安装mysql
```

上述命令会安装以下包：
apparmor
mysql-client-5.7
mysql-common
mysql-server
mysql-server-5.7
mysql-server-core-5.7
因此无需再安装mysql-client等。安装过程会提示设置mysql root用户的密码，设置完成后等待自动安装即可。默认安装完成就启动了mysql。

- 启动和关闭mysql服务器：

  ```
  service mysql start
  service mysql stop
  ```

![1680850535731](/images/spark/78.png)

- 确认是否启动成功，mysql节点处于LISTEN状态表示启动成功：

  ```
  sudo netstat -tap | grep mysql
  ```

  ![1680850589215](/images/spark/79.png)

- 进入mysql shell界面：

```
mysql -u root -p
```

![1680850651984](/images/spark/80.png)

解决利用sqoop导入MySQL中文乱码的问题（可以插入中文，但不能用sqoop导入中文）
导致导入时中文乱码的原因是character_set_server默认设置是latin1，如下图。

```
show variables like "char%";
```

![未修改server 编码](/images/spark/82.png)

可以单个设置修改编码方式`set character_set_server=utf8;`但是重启会失效，建议按以下方式修改编码方式。
(1)编辑配置文件。`sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`
(2)在[mysqld]下添加一行`character_set_server=utf8`。如下图

![未修改server 编码](/images/spark/83.png)

(3)重启MySQL服务。`service mysql restart`
(4)登陆MySQL，并查看MySQL目前设置的编码。`show variables like "char%";`

![未修改server 编码](/images/spark/84.png)

但是我的就直接是这样子的了：

![1680850651984](/images/spark/81.png)

## 二、MySQL常用操作

注意：MySQL中每个命令后都要以英文分号；结尾。
1、显示数据库
mysql> show databases;
MySql刚安装完有两个数据库：mysql和test。mysql库非常重要，它里面有MySQL的系统信息，我们改密码和新增用户，实际上就是用这个库中的相关表进行操作。

2、显示数据库中的表
mysql> use mysql; （打开库，对每个库进行操作就要打开此库）
Database changed
mysql> show tables;

3、显示数据表的结构：
describe 表名;

4、显示表中的记录：
select * from 表名;
例如：显示mysql库中user表中的纪录。所有能对MySQL用户操作的用户都在此表中。
select * from user;

5、建库：
create database 库名;
例如：创建一个名字位aaa的库
mysql> create database aaa;

6、建表：
use 库名；
create table 表名 (字段设定列表)；
例如：在刚创建的aaa库中建立表person,表中有id(序号，自动增长)，xm（姓名）,xb（性别）,csny（出身年月）四个字段
use aaa;
mysql> create table person (id int(3) auto_increment not null primary key, xm varchar(10),xb varchar(2),csny date);
可以用describe命令察看刚建立的表结构。
mysql> describe person;

![未修改server 编码](/images/spark/85.png)

7、增加记录
例如：增加几条相关纪录。
mysql>insert into person values(null,'张三','男','1997-01-02');
mysql>insert into person values(null,'李四','女','1996-12-02');
注意，字段的值（'张三','男','1997-01-02'）是使用两个英文的单撇号包围起来，后面也是如此。
因为在创建表时设置了id自增，因此无需插入id字段，用null代替即可。
可用select命令来验证结果。
mysql> select * from person;

![未修改server 编码](/images/spark/86.png)

8、修改纪录
例如：将张三的出生年月改为1971-01-10
mysql> update person set csny='1971-01-10' where xm='张三';

9、删除纪录
例如：删除张三的纪录。
mysql> delete from person where xm='张三';

10、删库和删表
drop database 库名;
drop table 表名；

11、查看mysql版本
在mysql5.0中命令如下：
show variables like 'version';
或者：select version();

https://dblab.xmu.edu.cn/blog/1002/