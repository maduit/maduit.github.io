---
title: 实验4  Spark SQL编程初级实践
tag: Spark
categories: 2023
---

## 1．Spark SQL基本操作

将下列JSON格式数据复制到Linux系统中，并保存命名为employee.json。



为employee.json创建DataFrame，并写出Python语句完成下列操作：

(1)     查询所有数据；

(2)     查询所有数据，并去除重复的数据；

(3)     查询所有数据，打印时去除id字段；

(4)     筛选出age>30的记录；

(5)     将数据按age分组；

(6)     将数据按name升序排列；

(7)     取出前3行数据；

(8)     查询所有记录的name列，并为其取别名为username；

(9)     查询年龄age的平均值；

(10)  查询年龄age的最小值。

```
首先为employee.json创建DataFrame，并写出Python语句完成下列操作：
创建DataFrame
答案：
>>> spark=SparkSession.builder().getOrCreate()
>>> df = spark.read.json("file:///usr/local/spark/employee.json")
(1)	查询DataFrame的所有数据
答案：>>> df.show()
(2)	查询所有数据，并去除重复的数据
答案：>>> df.distinct().show()
(3)	查询所有数据，打印时去除id字段
答案：>>> df.drop("id").show()
(4)	筛选age>20的记录
答案：>>> df.filter(df.age > 30 ).show()
(5)	将数据按name分组
答案：>>> df.groupBy("name").count().show()
(6)	将数据按name升序排列
答案：>>> df.sort(df.name.asc()).show()
(7)取出前3行数据
答案：>>> df.take(3) 或python> df.head(3)
(8)查询所有记录的name列，并为其取别名为username
答案：>>> df.select(df.name.alias("username")).show()
(9)查询年龄age的平均值
答案：>>> df.agg({"age": "mean"}).show()
(10)查询年龄age的最大值
答案：>>> df.agg({"age": "max"}).show()

```











## 2．编程实现将RDD转换为DataFrame

源文件内容如下（包含id,name,age）：



​       请将数据复制保存到Linux系统中，命名为employee.txt，实现从RDD转换得到DataFrame，并按“id:1,name:Ella,age:36”的格式打印出DataFrame的所有数据。请写出程序代码。





假设当前目录为/usr/local/spark/mycode/rddtodf，在当前目录下新建一个目录mkdir -p src/main/python，然后在目录/usr/local/spark/mycode/rddtodf/src/main/python下新建一个rddtodf.py，复制下面代码；（下列两种方式任选其一）

方法一：利用反射来推断包含特定类型对象的RDD的schema，适用对已知数据结构的RDD转换；

```
from pyspark.conf import SparkConf
from pyspark.sql.session import SparkSession
from pyspark import SparkContext
from pyspark.sql.types import Row
if __name__ == "__main__":
sc = SparkContext("local","Simple App")
peopleRDD = sc.textFile("file:///usr/local/spark/employee.txt")
rowRDD = peopleRDD.map(lambda line : line.split(",")).map(lambda attributes : Row(int(attributes[0]),attributes[1],int(attributes[2]))).toDF()
rowRDD.createOrReplaceTempView("employee")
personsDF = spark.sql("select * from employee")
personsDF.rdd.map(lambda t : "id:"+str(t[0])+","+"Name:"+t[1]+","+"age:"+str(t[2])).foreach(print)

```

方法二：使用编程接口，构造一个schema并将其应用在已知的RDD上。

```
from pyspark.sql.types import Row
from pyspark.sql.types import StructType
from pyspark.sql.types import StructField
from pyspark.sql.types import StringType
from pyspark.conf import SparkConf
from pyspark import SparkContext
from pyspark.sql.session import SparkSession
if __name__ == "__main__":
sc = SparkContext("local","Simple App")
peopleRDD = sc.textFile("file:///usr/local/spark/employee.txt")
schemaString = "id name age"
fields = list(map( lambda fieldName : StructField(fieldName, StringType(), nullable = True), schemaString.split(" ")))
schema = StructType(fields)
rowRDD = peopleRDD.map(lambda line : line.split(",")).map(lambda attributes : Row(int(attributes[0]),attributes[1],int(attributes[2])))
employeeDF = spark.createDataFrame(rowRDD, schema)
employeeDF.createOrReplaceTempView("employee")
results = spark.sql("SELECT * FROM employee")
results.rdd.map(lambda t : "id:"+str(t[0])+","+"Name:"+t[1]+","+"age:"+str(t[2])).foreach(print)

```

```
`python3 ./ rddtodf.py`
```



## 3. 编程实现利用DataFrame读写MySQL的数据

（1）在MySQL数据库中新建数据库sparktest，再创建表employee，包含如表5-2所示的两行数据。

**表****5-2 employee****表原有数据**

| id   | name  | gender | Age  |
| ---- | ----- | ------ | ---- |
| 1    | Alice | F      | 22   |
| 2    | John  | M      | 25   |

 ```
mysql> create database sparktest;
mysql> use sparktest;
mysql> create table employee (id int(4), name char(20), gender char(4), age int(4));
mysql> insert into employee values(1,'Alice','F',22);
mysql> insert into employee values(2,'John','M',25);

 ```



（2）配置Spark通过JDBC连接数据库MySQL，编程实现利用DataFrame插入如表5-3所示的两行数据到MySQL中，最后打印出age的最大值和age的总和。

**表****5-3 employee****表新增数据**

| id   | name | gender | age  |
| ---- | ---- | ------ | ---- |
| 3    | Mary | F      | 26   |
| 4    | Tom  | M      | 23   |

```
答案：假设当前目录为/usr/local/spark/mycode/testmysql，在当前目录下新建一个目录mkdir -p src/main/python，然后在目录/usr/local/spark/mycode/testmysql/src/main/python下新建一个testmysql.py，复制下面代码；
```

```
from pyspark import SparkContext
from pyspark.sql import SQLContext
from pyspark.sql.types import Row
from pyspark.sql.types import StructType
from pyspark.sql.types import StructField
from pyspark.sql.types import StringType
from pyspark.sql.types import IntegerType
if __name__ == "__main__":

sc = SparkContext( 'local', 'test')
spark=SQLContext(sc)
jdbcDF=spark.read.format("jdbc").option("url","jdbc:mysql://localhost:3306/sparktest").option("driver","com.mysql.jdbc.Driver").option("dbtable","employee").option("user", "root").option("password", "123").load()
jdbcDF.filter(jdbcDF.age>20).collect()//检测是否连接成功
studentRDD = sc.parallelize(["3 Mary F 26","4 Tom M 23"]).map(lambda line : line.split(" "))
schema = StructType([StructField("id",IntegerType(),True),StructField("name", StringType(), True),StructField("gender", StringType(), True),StructField("age",IntegerType(), True)])
rowRDD = studentRDD.map(lambda p : Row(int(p[0]),p[1].strip(), p[2].strip(),int(p[3])))
employeeDF = spark.createDataFrame(rowRDD, schema)
prop = {}
prop['user'] = 'root'
prop['password'] = '123'
prop['driver'] = "com.mysql.jdbc.Driver"
employeeDF.write.jdbc("jdbc:mysql://localhost:3306/sparktest",'employee','append', prop)
  jdbcDF.collect()
  jdbcDF.agg({"age": "max"}).show()
  jdbcDF.agg({"age": "sum"}).show()


```

然后我们，执行以下指令

```
 
python3 ./ rddtodf.py
 
```

在终端下，我们就可以看到结果了。

 