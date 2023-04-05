---
title: Spark入门：初级编程实践
tag: Spark
categories: 2023
---

1.pyspark交互式编程![1679922636425](E:\BLOG\blog1\source\images\spark\76.png)

（1）      该系总共有多少学生；

```
lines =   sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt")   
res = lines.map(lambda x:x.split(",")).map(lambda x: x[0]) //获取每行数据的第1列  
distinct_res = res.distinct()  //去重操作  

distinct_res.count()//取元素总个数   //265
```





答案为：265人

（2）      该系共开设了多少门课程；

```
lines =   sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt") 
res = lines.map(lambda x:x.split(",")).map(lambda x:x[1]) //获取每行数据的第2列
distinct_res = res.distinct()//去重操作  
distinct_res.count()//取元素总个数   //8   
```





答案为8门

（3）      Tom同学的总成绩平均分是多少；

```
lines =   sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt")
res = lines.map(lambda x:x.split(",")).filter(lambda   x:x[0]=="Tom") //筛选Tom同学的成绩信息 
res.foreach(print)   
score = res.map(lambda x:int(x[2])) //提取Tom同学的每门成绩，并转换为int类型   
num = res.count() //Tom同学选课门数  
sum_score = score.reduce(lambda x,y:x+y) //Tom同学的总成绩   avg = sum_score/num // 总成绩/门数=平均分
print(avg)   //30.8   
```

Tom同学的平均分为30.8分

（4）      求每名同学的选修的课程门数；

```
lines =   sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt") 
res = lines.map(lambda x:x.split(",")).map(lambda x:(x[0],1)) //学生每门课程都对应(学生姓名,1)，学生有n门课程则有n个(学生姓名,1)  
each_res = res.reduceByKey(lambda x,y: x+y) //按学生姓名获取每个学生的选课总数 
each_res.foreach(print)   
```

答案共265行

('Lewis', 4)

('Mike', 3)

('Walter', 4)

('Conrad', 2)

('Borg', 4)

……

（5）      该系DataBase课程共有多少人选修；

```
lines = sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt")  
res = lines.map(lambda x:x.split(",")).filter(lambda   x:x[1]=="DataBase")  
res.count()   //126   
```

答案为126人

（6）      各门课程的平均分是多少；

```
lines = sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt")  
res = lines.map(lambda x:x.split(",")).map(lambda   x:(x[1],(int(x[2]),1))) //为每门课程的分数后面新增一列1，表示1个学生选择了该课程。格式如('ComputerNetwork',   (44, 1))
temp = res.reduceByKey(lambda x,y:(x[0]+y[0],x[1]+y[1])) //按课程名聚合课程总分和选课人数。格式如('ComputerNetwork',   (7370, 142))   
avg = temp.map(lambda x:(x[0], round(x[1][0]/x[1][1],2)))//课程总分/选课人数 = 平均分，并利用round(x,2)保留两位小数  
avg.foreach(print)   
```

答案为：

('ComputerNetwork', 51.9)

('Software', 50.91)

('DataBase', 50.54)

('Algorithm', 48.83)

('OperatingSystem', 54.94)

('Python', 57.82)

('DataStructure', 47.57)

('CLanguage', 50.61)

（7）使用累加器计算共有多少人选了DataBase这门课。

```
lines = sc.textFile("file:///usr/local/spark/sparksqldata/Data01.txt") 
res = lines.map(lambda x:x.split(",")).filter(lambda   x:x[1]=="DataBase")//筛选出选了DataBase课程的数据  
accum = sc.accumulator(0) //定义一个从0开始的累加器accum  
res.foreach(lambda x:accum.add(1))//遍历res，每扫描一条数据，累加器加1 
accum.value //输出累加器的最终值   
//126   
```



## 2.编写独立应用程序实现数据去重

对于两个输入文件A和B，编写Spark独立应用程序，对两个文件进行合并，并剔除其中重复的内容，得到一个新文件C。下面是输入文件和输出文件的一个样例，供参考。

输入文件A的样例如下：

20170101    x

20170102    y

20170103    x

20170104    y

20170105    z

20170106    z

输入文件B的样例如下：

20170101    y

20170102    y

20170103    x

20170104    z

20170105    y

根据输入的文件A和B合并得到的输出文件C的样例如下：

20170101    x

20170101    y

20170102    y

20170103    x

20170104    y

20170104    z

20170105    y

20170105    z

20170106    z





【参考答案】

 

  实验答案参考步骤如下：

```
（1）假设当前目录为/usr/local/spark/mycode/remdup，在当前目录下新建一个remdup.py文件，复制下面代码；
```



```
（2）最后在目录/usr/local/spark/mycode/remdup下执行下面命令执行程序（注意执行程序时请先退出pyspark shell，否则会出现“地址已在使用”的警告）；
$ python3 remdup.py
（3）在目录/usr/local/spark/mycode/remdup/result下即可得到结果文件part-00000。
```

