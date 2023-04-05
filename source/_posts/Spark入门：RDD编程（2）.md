---
title: Spark入门：RDD编程（2）
tag: Spark
categories: 2023
---

# Spark入门：RDD编程(2)

# 4.2键值对RDD

### 4.2.1键值对RDD的创建

![1679919962655](E:\BLOG\blog1\source\images\spark\55.png)



```
lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")

pairRdd=lines.flatMap(lambda line:line.split(" ")).map(lambda word:(word,1))

pairRdd.foreach(print)

```



![1679920610754](E:\BLOG\blog1\source\images\spark\57.png)

![1679920024570](E:\BLOG\blog1\source\images\spark\56.png)

![1679920857471](E:\BLOG\blog1\source\images\spark\58.png)

### 4.2.2常用键值对转换操作

![1679920904927](E:\BLOG\blog1\source\images\spark\59.png)

#### 1.reduceByKey(func)

![1679920937895](E:\BLOG\blog1\source\images\spark\60.png)

![1679921092076](E:\BLOG\blog1\source\images\spark\61.png)

#### 2.groupByKey()

![1679921126885](E:\BLOG\blog1\source\images\spark\62.png)

![1679921148329](E:\BLOG\blog1\source\images\spark\63.png)

![1679921162978](E:\BLOG\blog1\source\images\spark\64.png)

#### 3.key

![1679921231527](E:\BLOG\blog1\source\images\spark\65.png)

#### 4.value

![1679921285079](E:\BLOG\blog1\source\images\spark\66.png)

#### 5.sortByKey()

![1679921328845](E:\BLOG\blog1\source\images\spark\67.png)



#### 6.sortBy()



![1679921389676](E:\BLOG\blog1\source\images\spark\68.png)

#### 7.mapValues(func)

![1679921487925](E:\BLOG\blog1\source\images\spark\69.png)

#### 8.join

![1679921512081](E:\BLOG\blog1\source\images\spark\70.png)

### 4.2.3一个综合实例

![1679921667681](E:\BLOG\blog1\source\images\spark\71.png)