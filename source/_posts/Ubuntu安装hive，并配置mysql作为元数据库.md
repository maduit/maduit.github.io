---
title: Ubuntu安装hive，并配置mysql作为元数据库
tag: Spark
categories: 2023
---

hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的sql查询功能，可以将sql语句转换为MapReduce任务进行运行。 其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。

## 一、安装hive

**1. 下载并解压hive源程序**

[Hive下载地址](http://www.apache.org/dyn/closer.cgi/hive/)

```
sudo tar -zxvf ./apache-hive-1.2.1-bin.tar.gz -C /usr/local   # 解压到/usr/local中
cd /usr/local/
sudo mv apache-hive-1.2.1-bin hive       # 将文件夹名改为hive
sudo chown -R dblab:dblab hive            # 修改文件权限
```

**2. 配置环境变量**
为了方便使用，我们把hive命令加入到环境变量中去，编辑~/.bashrc文件`vim ~/.bashrc`，在最前面一行添加:

```
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
```

保存退出后，运行`source ~/.bashrc`使配置立即生效。

**3. 修改/usr/local/hive/conf下的hive-site.xml**
将hive-default.xml.template重命名为hive-default.xml；新建一个文件`touch hive-site.xml`，并在hive-site.xml中粘贴如下配置信息：

```
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
  <property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
    <description>JDBC connect string for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionDriverName</name>
    <value>com.mysql.jdbc.Driver</value>
    <description>Driver class name for a JDBC metastore</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionUserName</name>
    <value>hive</value>
    <description>username to use against metastore database</description>
  </property>
  <property>
    <name>javax.jdo.option.ConnectionPassword</name>
    <value>hive</value>
    <description>password to use against metastore database</description>
  </property>
</configuration>
```

## 二、安装并配置mysql

**1.Ubuntu下mysql的安装请参考**：[Ubuntu安装MySQL](https://dblab.xmu.edu.cn/blog/install-mysql/)
**2.下载mysql jdbc 包**,[下载地址](http://www.mysql.com/downloads/connector/j/)

https://downloads.mysql.com/archives/c-j/

```
tar -zxvf mysql-connector-java-5.1.40.tar.gz   #解压
cp mysql-connector-java-5.1.40/mysql-connector-java-5.1.40-bin.jar  /usr/local/hive/lib #将mysql-connector-java-5.1.40-bin.jar拷贝到/usr/local/hive/lib目录下
```

**3. 启动并登陆mysql shell**

```
 service mysql start #启动mysql服务
 mysql -u root -p  #登陆shell界面
```

**4. 新建hive数据库**。

```
mysql> create database hive;    #这个hive数据库与hive-site.xml中localhost:3306/hive的hive对应，用来保存hive元数据
```

**5. 配置mysql允许hive接入：**

```
这条MySQL语句是为用户'hive'授予所有数据库的所有权限，并设置'hive'用户的密码为'hive'。这个命令通常用于在授权给用户访问数据库之前，先为该用户创建一个数据库账户并为其授权。

在这个命令中，'identified by'语句用于设置用户的密码。'hive'是数据库账户的用户名和密码，它将用于在hive-site.xml配置文件中连接数据库。

请注意，为了安全起见，建议不要在授权语句中明文指定密码。相反，可以使用以下语句来创建用户并设置其密码：

CREATE USER 'hive'@'localhost' IDENTIFIED BY 'password';
然后使用以下命令来授予用户所有数据库的所有权限：

GRANT ALL ON *.* TO 'hive'@'localhost';
这样做可以保护您的数据库免受潜在的安全威胁。
```



```
mysql> grant all on *.* to hive@localhost identified by 'hive';   #将所有数据库的所有表的所有权限赋给hive用户，后面的hive是配置hive-site.xml中配置的连接密码
mysql> flush privileges;  #刷新mysql系统权限关系表
```

**6. 启动hive**
启动hive之前，请先启动hadoop集群。

```
start-all.sh #启动hadoop
hive  #启动hive
```

解决Hive启动，Hive metastore database is not initialized的错误。出错原因：重新安装Hive和MySQL，导致版本、配置不一致。在终端执行如下命令:

```
schematool -dbType mysql -initSchema
```

Hive 分布现在包含一个用于 Hive Metastore 架构操控的脱机工具，名为 schematool.此工具可用于初始化当前 Hive 版本的 Metastore 架构。此外，其还可处理从较旧版本到新版本的架构升级。

https://dblab.xmu.edu.cn/blog/996/