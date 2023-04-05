---
title: Spark入门：RDD编程
tag: Spark
categories: 2023
---
# Spark入门：RDD编程
<!-- more -->
## RDD创建

RDD可以通过两种方式创建：
* 第一种：读取一个外部数据集。比如，从本地文件加载数据集，或者从HDFS文件系统、HBase、Cassandra、Amazon S3等外部数据源中加载数据集。Spark可以支持文本文件、SequenceFile文件（Hadoop提供的 SequenceFile是一个由二进制序列化过的key/value的字节流组成的文本存储文件）和其他符合Hadoop InputFormat格式的文件。
* 第二种：调用SparkContext的parallelize方法，在Driver中一个已经存在的集合（数组）上创建

在即将进行相关的实践操作之前，我们首先要登录Linux系统（本教程统一采用hadoop用户登录），然后，打开命令行“终端”，请按照下面的命令启动Hadoop中的HDFS组件：

### 创建RDD之前的准备工作

在即将进行相关的实践操作之前，我们首先要登录Linux系统（本教程统一采用hadoop用户登录），然后，打开命令行“终端”，请按照下面的命令启动Hadoop中的HDFS组件：

```
cd  /usr/local/hadoop
./sbin/start-dfs.sh
```

然后，我们按照下面命令启动spark-shell：



```
cd /usr/local/spark
./bin/pyspark
```

![1678715171189](/images/spark/1.png)

然后，新建第二个“终端”，方法是，在前面已经建设的第一个终端窗口的左上方，点击“终端”菜单，在弹出的子菜单中选择“新建终端”，就可以打开第二个终端窗口，现在，我们切换到第二个终端窗口，在第二个终端窗口中，执行以下命令，进入之前已经创建好的“/usr/local/spark/mycode/”目录，在这个目录下新建rdd子目录，用来存放本章的代码和相关文件：

```
cd usr/local/spark/mycode/
mkdir rdd
```

然后，使用vim编辑器，在rdd目录下新建一个word.txt文件，你可以在文件里面随便输入几行英文语句用来测试。

经过上面的准备工作以后，我们就可以开始创建RDD了。



### 从文件系统中加载数据创建RDD

Spark采用textFile()方法来从文件系统中加载数据创建RDD，该方法把文件的URI作为参数，这个URI可以是本地文件系统的地址，或者是分布式文件系统HDFS的地址，或者是Amazon S3的地址等等。
下面请切换回spark-shell窗口，看一下如何从本地文件系统中加载数据：

```
>>>lines = sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")

```



![1678715444810](/images/spark/2.png)





```
>>>lines.foreach(print)
```





![1678715770877](/images/spark/3.png)

![1678720839060](/images/spark/14.png)

### 加载HDFS中的文件

为了能够读取HDFS中的文件，请首先启动Hadoop中的HDFS组件。注意，之前我们在“Spark安装”这章内容已经介绍了如何安装Hadoop和Spark，所以，这里我们可以使用以下命令直接启动Hadoop中的HDFS组件（由于用不到MapReduce组件，所以，不需要启动MapReduce或者YARN）。请到第二个终端窗口，使用Linux Shell命令提示符状态，然后输入下面命令：

```
cd /usr/local/hadoop
./sbin/start-dfs.sh
```



![1678715996913](/images/spark/4.png)



启动结束后，HDFS开始进入可用状态。如果你在HDFS文件系统中，还没有为当前Linux登录用户创建目录(本教程统一使用用户名hadoop登录Linux系统)，请使用下面命令创建：

```
./bin/hdfs dfs -mkdir -p /user/hadoop
```

也就是说，HDFS文件系统为Linux登录用户开辟的默认目录是“/user/用户名”（注意：是user，不是usr），本教程统一使用用户名hadoop登录Linux系统，所以，上面创建了“/user/hadoop”目录，再次强调，这个目录是在HDFS文件系统中，不在本地文件系统中。创建好以后，下面我们使用命令查看一下HDFS文件系统中的目录和文件：

![1678716107634](/images/spark/5.png)

```
./bin/hdfs dfs -ls .
```

** 但这个命令我搞不出来，显示的是这个

![1678717090642](/images/spark/6.png)

--

上面命令中，最后一个点号“.”，表示要查看Linux当前登录用户hadoop在HDFS文件系统中与hadoop对应的目录下的文件，也就是查看HDFS文件系统中“/user/hadoop/”目录下的文件，所以，下面两条命令是等价的：

--

```
./bin/hdfs dfs -ls .
./bin/hdfs dfs -ls /user/hadoop
```

 你自己可以试一试，只有第二个可以用

![1678717187423](/images/spark/7.png)

如果要查看HDFS文件系统根目录下的内容，需要使用下面命令：

```
./bin/hdfs dfs -ls /
```

然后输出的东西见上面那个图，有一个items

下面，我们把本地文件系统中的“/usr/local/spark/mycode/rdd/word.txt”上传到分布式文件系统HDFS中（放到hadoop用户目录下）：

```
./bin/hdfs dfs -put /usr/local/spark/mycode/rdd/word.txt /user/hadoop
```

![1678717295097](/images/spark/8.png)

然后，用命令查看一下HDFS的hadoop用户目录下是否多了word.txt文件，可以使用下面命令列出hadoop目录下的内容：

```
./bin/hdfs dfs -ls /
```

![1678717376742](/images/spark/9.png)

可以看到，确实多了一个word.txt文件，我们使用cat命令查看一个HDFS中的word.txt文件的内容，命令如下：

```
./bin/hdfs dfs -cat ./word.txt
```

![1678717420477](/images/spark/10.png)

上面命令执行后，就会看到HDFS中word.txt的内容了。

现在，让我们切换回到spark-shell窗口，编写语句从HDFS中加载word.txt文件，并显示第一行文本内容：

```
>>>lines= sc.textFile("hdfs://localhost:9000/user/hadoop/word.txt")
>>>lines=sc.foreach(print)
```

![1678717605626](/images/spark/11.png)

![1678717639868](/images/spark/12.png)

注意，上面三条命令是完全等价的命令，只不过使用了不同的目录形式，你可以使用其中任意一条命令完成数据加载操作。

在使用Spark读取文件时，需要说明以下几点：
（1）如果使用了本地文件系统的路径，那么，必须要保证在所有的worker节点上，也都能够采用相同的路径访问到该文件，比如，可以把该文件拷贝到每个worker节点上，或者也可以使用网络挂载共享文件系统。
（2）textFile()方法的输入参数，可以是文件名，也可以是目录，也可以是压缩文件等。比如，textFile("/my/directory"), textFile("/my/directory/*.txt"), and textFile("/my/directory/*.gz").
（3）textFile()方法也可以接受第2个输入参数（可选），用来指定分区的数目。默认情况下，Spark会为HDFS的每个block创建一个分区（HDFS中每个block默认是128MB）。你也可以提供一个比block数量更大的值作为分区数目，但是，你不能提供一个小于block数量的值作为分区数目。

### 通过并行集合（数组）创建RDD

可以调用SparkContext的parallelize方法，在Driver中一个已经存在的集合（数组）上创建。
下面请在spark-shell中操作：

```
>>>array = [1,2,3,4,5]
>>>rdd = sc.parallelize(array)
>>>rdd.foreach(print)
```

![1678720471787](/images/spark/13.png)

![1678720865514](/images/spark/15.png)

## RDD操作

RDD被创建好以后，在后续使用过程中一般会发生两种操作：
\*  转换（Transformation）： 基于现有的数据集创建一个新的数据集。
\*  行动（Action）：在数据集上进行运算，返回计算值。

### 转换操作

对于RDD而言，每一次转换操作都会产生不同的RDD，供给下一个“转换”使用。转换得到的RDD是惰性求值的，也就是说，整个转换过程只是记录了转换的轨迹，并不会发生真正的计算，只有遇到行动操作时，才会发生真正的计算，开始从血缘关系源头开始，进行物理的转换操作。
下面列出一些常见的转换操作（Transformation API）：
\* filter(func)：筛选出满足函数func的元素，并返回一个新的数据集
\* map(func)：将每个元素传递到函数func中，并将结果返回为一个新的数据集
\* flatMap(func)：与map()相似，但每个输入元素都可以映射到0或多个输出结果
\* groupByKey()：应用于(K,V)键值对的数据集时，返回一个新的(K, Iterable)形式的数据集
\* reduceByKey(func)：应用于(K,V)键值对的数据集时，返回一个新的(K, V)形式的数据集，其中的每个值是将每个key传递到函数func中进行聚合

![1678720959522](/images/spark/16.png)



![1678720974857](/images/spark/17.png)

####  1.filter(func)

![1678720994986](/images/spark/18.png)





好吧，这个书上说要把word.txt写成

```
Hadoop is good

Spark is fast

Spark is better
```

那我们就

```
cd /usr/local/spark/mycode/rdd
```

```
gedit word.txt
```

然后再

```
>>>lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
>>>linesWithSpark = lines.filter(lambda line: "Spark" in line)
>>>linesWithSpark.foreach(print)
```



![1678722624927](/images/spark/20.png)

上面的代码中，lines就是一个RDD。lines.filter()会遍历lines中的每行文本，并对每行文本执行括号中的匿名函数，也就是执行Lamda表达式：line => line.contains("Spark")，在执行Lamda表达式时，会把当前遍历到的这行文本内容赋值给参数line，然后，执行处理逻辑line.contains("Spark")，也就是只有当改行文本包含“Spark”才满足条件，才会被放入到结果集中。最后，等到lines集合遍历结束后，就会得到一个结果集，这个结果集中包含了所有包含“Spark”的行。最后，对这个结果集调用count()，这是一个行动操作，会计算出结果集中的元素个数。

#### 2.map(func)

![1678724709366](/images/spark/21.png)

map(func)：将每个元素传递到函数func中，并将结果返回为一个新的数据集

```
>>> data =[1,2,3,4,5]
>>> rdd1 = sc.parallelize(data)
>>> rdd2 = rdd1.map(lambda x:x+10)
>>> rdd2.foreach(print)
```

![1678724836563](/images/spark/22.png)

上述语句执行过程如图 所示。第 1行语句创建了一个包含 5 个整型元素的列表 data。第2行语句执行 sc.parallelize(data)，从列表 data 中生成一个 RDD，即 rdd1,rdd1 中包含了5 个整型的元素即1、2、3、4、5。第 3 行语句执行 rdd1.map0操作,map0的输入参数“lambda x:x+10”是一个Lambda表达式。rdd1.map(lambda x:x+10)的含义是，依次取出 rdd1 这个RDD 中的每个元素，对于当前取到的元素，把它赋值给 Lambda 表达式中的变量x，然后，执行 Lambda 表达式的函数体部分“x+10”也就是把变量x的值和 10 相加后，作为函数的返回值，并作为一个元素放入到新的 RDD(即rdd2中。最终，新牛成的RDD (即 rdd2) 中包含了 5 个整型元素，即 11、12、13、14、15。

另外一个实例：


```
>>>lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
>>> words = lines.map(lambda line:line.split(" "))
>>> words.foreach(print)
```

![1678725201907](/images/spark/24.png)

上述语句执行过程如图所示。在第 1 行语句中，执行 sc.textFile0方法把 word.txt 文件中的数据加载到内存生成一个 RDD，即 lines，这个RDD 中的每个元素都是字符串类型，即每个 RDD 元素都是一行文本，比如，lines 中的第 1 个元素是"Hadoop is good"，第2 个元素是"Spark is fast"，第3个元素是"Spark is better"。在第 2 行语句中，执行 lies.map0操作，map0的输入参数 lambdaline:line.split(")是一个 Lambda 表达式。linesmap(lambda line:line.split("")的含义是，依次取出 lines这个 RDD 中的每个元素，对于当前取到的元素，把它赋值给 Lambda 表达式中的变量 line，然后，执行 Lambda 表达式的函数体部分 line.split("")。因为 line 是一行文本，如"Hadoop is good"，一行文本中包含了很多个单词，单词之间以空格进行分隔，所以，line.split(""的功能是，以空格作为分隔符把 line 拆分成一个个单词,拆分后得到的单词都封装在一个列表对象中,成为新的 RDD( 即 words)的一个元素，比如，"Hadoop is good"被拆分后，得到"Hadoop"、"is"和"good"3 个单词，会被封装到一个列表对象中，即["Hadoop"."is","good”]，成为 words 这个 RDD 中的一个元素。

![1678725073888](/images/spark/23.png)

#### 3.flatMap(func)

flatMap(func)与 map0相似，但每个输入元素都可以映射到 0 或多个输出结果。例如:



```
>>>lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
>>>words =lines.flatMap(lambda line:line.split(" "))
>>> words.foreach(print)
```

![1678725603008](/images/spark/26.png)

上述语句执行过程如图所示。在第 1行语句中，执行 sc.textFile0方法把 wordtxt 文件中的数据加载到内存生成一个RDD，即 lines，这个 RDD 中的每个元素都是字符串类型，即每个RDD 元素都是一行文本。在第2行语句中，执行 linesflatMap0操作，flatMap0的输入参数 line:linesplit("")是个Lambda 表达式。lines.flatMap(lambda line:line.split(""))的结果，等价于如下两步操作的结果。

![1678725419517](/images/spark/25.png)

第1步: map0。执行 lines.map(lambda line: linesplit(""))操作，从 lines 转换得到一个新的 RDD(即wordArray),wordArray 中的每个元素都是一个列表,比如,第1个元素是["Hadoop","is"，"good"]7.第2个元素是"Spark"，"is","fast”]，第3 个元素是["Spark","is""better”]。

第 2步:拍扁 ( flat)。flatMap0操作中的“flat”是一个很形象的动作-“拍扁”，也就是把vordArray 中的每个 RDD 元素都“拍扁”成多个元素。所有这些被拍扁以后得到的元素，构成一个新的 RDD，即 words。比如，wordArray 中的第1个元素是["Hadoop","is","good]，被拍扁以后得到3个新的字符串类型的元素,即"Hadoop”、"is"和"good”; wordArray 中的第 2 个元素是["Spark","is"fast"],被拍扁以后得到 3 个新的元素,即"Spark"、"is"和"fast”; wordArray 中的第 3 个元素是["Spark""is""beter”，被拍扁以后得到 3 个新的元素，即"Spark”、"is"和"beter"。最终，这些被拍扁以后得到的 9 个字符串类型的元素构成一个新的 RDD (即 words )。也就是说，words 里面包含了 9 个字串类型的元素，分别是"Hadoop"、"is"、"good"、"Spark"、"is"、"fast"、"Spark"、"is"和"。