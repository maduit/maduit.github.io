---
title: Spark入门：RDD编程（1）
tag: Spark
categories: 2023
---
# Spark入门：RDD编程(1)
<!-- more -->

# 4.1RDD编程基础

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

### Ⅰ转换操作

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

第 2步:拍扁 ( flat)。flatMap0操作中的“flat”是一个很形象的动作-“拍扁”，也就是把vordArray 中的每个 RDD 元素都“拍扁”成多个元素。所有这些被拍扁以后得到的元素，构成一个新的 RDD，即 words。比如，wordArray 中的第1个元素是["Hadoop","is","good]，被拍扁以后得到3个新的字符串类型的元素,即"Hadoop”、"is"和"good”; wordArray 中的第 2 个元素是["Spark","is"fast"],被拍扁以后得到 3 个新的元素,即"Spark"、"is"和"fast”; wordArray 中的第 3 个元素是["Spark""is""beter”，被拍扁以后得到 3 个新的元素，即"Spark”、"is"和"beter"。最终，这些被拍扁以后得到的 9 个字符串类型的元素构成一个新的 RDD (即 words )。也就是说，words 里面包含了 9 个字串类型的元素，分别是"Hadoop"、"is"、"good"、"Spark"、"is"、"fast"、"Spark"、"is"和"'better'。

#### 4.gropByKey

groupByKey()应用于(K,V)键值对的数据集时，返回一个新的(K,Iterable)形式的数据集。

```
words=sc.parallelize([("Hadoop",1),("is",1),("good",1),("Spark",1),("is",1),("better",1)])
words1=words.groupByKey()
words1.foreach(print)
```

![1679317892865](/images/spark/27.png)

![1679318485755](/images/spark/28.png)

![1679318770911](/images/spark/29.png)

如图所示，在这个实例中，名称为 words 的 RDD 中包含了 9 个元素，每个元素都是(KV)键值对类型。wordsl=words.groupByKey0操作执行以后，所有 key 相同的键值对，它们的 value都被归并到一起。比如，(“is",1)、(“is",1)、(is",1)这3 个键值对的 key 相同，就会被归并成一个新的键值对("is",(1,1,1))，其中，key 是"is"，value 是(1,1,1)，而且，value 会被封装成 Iterable 对象 (一种可选代集合 )。

#### 5.reduceByKey(func)

reduceByKey(func)应用于(KV)键值对的数据集时，返回一个新的(K,V)形式的数据集，其中的每个值是将每个key传递到函数func中进行聚合后得到的结果

![1679318833321](/images/spark/30.png)

```\
words=sc.parallelize([("Hadoop",1),("is",1),("good",1),("Spark",1),("is",1),("better",1)])
words1=words.reduceByKey(lambda a,b:a+b)
words1.foreach(print)
```

![1679319056594](/images/spark/31.png)

![1679319272140](/images/spark/33.png)

![1679319106659](/images/spark/32.png)



如图所示，在这个实例中，名称为 words 的 RDD 中包含了 9个元素，每个元素都是(K,V)键值对类型。words.reduceByKey(lambda a,b;atb)操作执行以后,所有 key 相同的键值对，它们的 value首先被归并到一起，比如，(“is",1)、(“is"1)、(“is",1)这3 个键值对的 key 相同，就会被归并成一个新的键值对("is",(1,1,1))，其中，key 是"is"，value 是一个 value-list，即(1,1,1)。然后，使用 func 函数把(l,1,1)聚合到一起，这里的 func 函数是一个Lambda 表达式，即 lambda a,b;atb，它的功能是把(1,1,1)这个 value-list 中的每个元素进行汇总求和。首先，把 value-list 中的第1个元素(即 1) 赋值给参数a，把 value-list 中的第 2个元素(也是 1)赋值给参数 b，执行 atb 得到 2，然后，继续对 value-list中的元素执行下一次计算，把刚才求和得到的 2 赋值给 a，把 value-list 中的第 3 个元素(即 1)赋值给b，再次执行 a+b 计算得到 3。最终，就得到聚合后的结果('is',3)。

### Ⅱ行动操作

行动操作是真正触发计算的地方。Spark程序执行到行动操作时，才会执行真正的计算，从文件中加载数据，完成一次又一次转换操作，最终，完成行动操作得到结果。

![1679319390149](/images/spark/34.png)

用一个例子看看

![1679319465685](/images/spark/35.png)

```
rdd =sc.parallelize([1,2,3,4,5])
rdd.count()
```

```5```

```\
rdd.first()

```

```1```

```
rdd.take(3)
```

```[1, 2, 3]```

```
rdd.reduce(lambda a,b:a+b)
```

```15```

```
rdd.collect()
```

```[1, 2, 3, 4, 5]```

```
rdd.foreach(lambda elem:print(elem))
```

```1
1
2
3
4
5
```

这里首先使用 sc.parallelize([1,2,3,4,5])生成了一个 RDD,变量名称为 rdd,rdd 中包含了5个元素分别是1、2、3、4和5，因此，rdd.count0语句执行以后返回的结果是 5。执行 rdd.first0语句后，会返回第1个元素，即1。当执行完 rdd.take(3)语句以后，会以列表的形式返回 rdd 中的前 3 个元素即[1,2,3]。执行完```rdd.reduce(lambda a,b:a+b)```语句后，会得到对 rdd 中的所有元素(即1、2、3、4、5进行求和以后的结果，即 15。在执行 rdd.reduce(lambda a,b;atb)时，系统会把 rdd 中的第1个元素1传入参数 a，把rdd 的第2个元素 2 传入参数 b，执行 a+b 计算得到求和结果 3;然后，把这个求和的结果 3 传入给参数 a，把 rdd 的第 3 个元素3 传入参数 b，执行 atb 计算得到求和结果 6; 然后,把6传入参数 a，把 rdd 的第 4 个元素 4 传入参数 b，执行 a+b 计算得到求和结果 10; 最后，把 10传入参数 a，把 rdd 的第 5个元素 5 传入参数 b，执行 atb 算得到求和结果 S。接下来，执行```rdd.collect()```，以列表的形式返回 rdd 中的所有元素，可以看出，执行结果是一个列表[1,2,3,4,5]。在这个实例的最后，执行了语句 ```rdd.foreach(lambda elem:print(elem)```，该语句会依次遍历 rdd 中的每个元素，把当前遍历到的元素赋值给变量 elem，并使用 print(elem)打印出 elem 的值。实际上```rdd.foreach(lambda elem:print(elem))```可以被简化成``` rdd.foreach(print)```，执行效果是一样的。
需要特别强调的是,当采用Local 模式在单机上执行时,```rdd.foreach(print)```语句会打印出一个RDD中的所有元素。但是，当采用集群模式执行时，在 Worker 节点上执行打印语句是输出到 Worker 节点的 stdout 中，而不是输出到任务控制节点 Driver 中，因此，任务控制节点 Driver 中的 stdout 是不会显示打印语句的这些输出内容的。为了能够把所有 Worker 节点上的打印输出信息也显示到 Driver中，就需要使用 collect0方法，比如，```print(rdd.collect())```。但是，由于 collect0方法会把各个 Worker节点上的所有 RDD元素都抓取到 Driver 中，因此，这可能会导致 Driver 所在节点发生内存溢出。所以，在实际编程中，需要谨慎使用``` collect()``方法。

### Ⅲ惰性机制

惰性机制是指整个转换过程只是记录了转换的轨迹，并不会发生真正的计算，只有遇到行动操作时，才会触发“从头到尾”的真正的计算。这里给出一段简单的语句来解释 Spark 的惰性机制。

![1679320607704](/images/spark/37.png)

```
lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
lineLengths=lines.map(lambda s:len(s))
totalLength=lineLengths.reduce(lambda a,b:a+b)
print(totalLength)
```

在上述语句中，第1 行语句中的 textFile()是一个转换操作，执行后，系统只会记录这次转换，并不会真正读取 wordtxt 文件的数据到内存中;第 2 行语句的 map也是一个转换操作，系统只是记录这次转换，不会真正执行 map()方法;第 3 行语句的 reduce()方法是一个“行动”类型的操作，这时，系统会生成一个作业，触发真正的计算。也就是说，这时才会加载 word.txt 的数据到内存，生成lines 这个RDD。lines 中的每个元素都是一行文本，然后，对 lines 执行 map()方法，计算这个RDD中每个元素的长度(即一行文本包含的单词个数 )，得到新的 RDD，即 lineLengths，这个RDD中每个元素都是整型，表示文本的长度。最后，在 lineLengths 上调用reduce()方法，执行 RDD元素求和，得到所有文本长度的总和。

## 持久化

在 Spark 中，RDD 采用惰性求值的机制，每次遇到行动操作，都会从头开始执行计算。每次调用行动操作，都会触发一次从头开始的计算，这对于迭代计算而言，代价是很大的，因为选代计算经常需要多次重复使用同一组数据。下面就是多次计算同一个 RDD 的例子。

![1679321131046](/images/spark/38.png)

```
list = ["Hadoop","Spark","Hive"]
rdd = sc.parallelize(list)
print (rdd.count ()) #行动操作，触发一次真正从头到尾的计算
print (','.join(rdd.collect())) #行动操作，触发一次真正从头到尾的计算Hadoop,Spark,Hive
```

![1679320828250](/images/spark/39.png)

实际上，可以通过持久化(缓存)机制来避免这种重复计算的开销。具体方法是使用 persist0方法将一个 RDD 标记为持久化，之所以要“标记为持久化”，是因为出现 persist0语句的地方，并不会马上计算生成 RDD 并把它持久化，而是要等到遇到第一个行动操作触发真正计算以后，才会把算结果进行持久化。持久化后的 RDD 将会被保留在计算节点的内存中，被后面的行动操作重复使用persist0的圆括号中包含的是持久化级别参数，可以有如下不同的级别。

* persist(MEMORY ONLY): 表示将 RDD作为反序列化的对象存储于JVM 中，如果内存足，就要按照 LRU 原则替换缓存中的内容。

* persist(MEMORYAND DISK):表示将RDD作为反序列化的对象存储在JVM中，如果内存不足，超出的分区将会被存放在硬盘上。

  一般而言，使用 cache()方法时，会调用 persist(MEMORY ONLY)。针对上面的实例，增加持久化语句以后的执行过程如下:

```
list = ["Hadoop","Spark","Hive"]
rdd = sc.parallelize(list)
rdd.cache() #会调用 persist(MEMORY ONLY)，但是，语句执行到这里，并不会缓存 rdd，因为这#时 rdd 还没有被计算生成
print(rdd.count()) #第一次行动操作，触发一次真正从头到尾的计算，这时上面的 rdd.cache ()#才会被执行，把这个 rdd 放到缓存中
print('.'.join(rdd.collect())) #第二次行动操作，不需要触发从头到尾的计算，只需要重复使#用上面缓存中的 rdd
```

![1679321297416](/images/spark/40.png)

持久化RDD 会占用内存空间，当不再需要一个 RDD 时，就可以使用 unpersist0方法手动地把持久化的 RDD 从缓存中移除，释放内存空间。

### 分区

#### 1.分区的作用

RDD 是弹性分布式数据集，通常 RDD 很大，会被分成很多个分区，分别保存在不同的节点上如图49所示，一个集群中包含 4 个工作节点( WorkerNode )，分别是 WorkerNodel、WorkerNode2WorkerNode3 和 WorkerNode4。假设有两个 RDD，即rdd1 和 rdd2，其中，rdd1 包含5 个分区(即plp2、p3、p4和p5)，rdd2 包含3 个分区(即p6、p7和p8)。
对RDD 进行分区，第一个作用是增加并行度。比如，在图 4-9 中，rdd2 的 3 个分区 p6、p7和p8,分布在3 个不同的工作节点 WorkerNode2、WorkerNode3 和 WorkerNode4 上，就可以在这3个T作节点上分别启动 3 个线程对这 3 个分区的数据进行并行处理，增加任务的并行度。

![1679321394814](/images/spark/41.png)

对 RDD 进行分区的第二个作用是减少通信开销。在分布式系统中，通信的代价是巨大的，控制数据分布以获得最少的网络传输可以极大地提升整体性能。Spark 程序可以通过控制 RDD 分区方式来减少网络通信的开销。下面通过一个实例来解释为什么通过分区可以减少网络传输开销。

连接(join)是查询分析中经常使用的一种操作。假设在某种应用中需要对两个表进行连接操作第1个表是一个很大的用户信息表 UserData(UserID,UserInfo)，其中，UserId 和 UserInfo 是 UserData表的两个字段，UserInfo 包含了某个用户所订阅的主题信息。第 2 个表是 Events(UserID,LinkInfo),这个表比较小，只记录了过去 5 分钟内发生的事件，即某个用户查看了哪个链接。为了对用户访问情况进行统计，需要周期性地对 UserData 和 Events 这两个表进行连接操作，获得(UserID,UserInfo,LinkInfo)这种形式的结果，从而知道某个用户订阅的是哪个主题，以及访问了哪个链接。

可以用 Spark 来实现上述应用场景。在执行 Spark 作业时，首先，UserData 表会被加载到内存中生成RDD(假设 RDD的名称为 userData)，RDD 中的每个元素是(UserID,UserInfo)这种形式的键值对,即 key 是 UserID,value 是 UserInfo;Events 表也会被加载到内存中生成RDD(假设名称为 events)RDD中的每个元素是(UserID，LinkInfo)这种形式的键值对，key 是 UserID，value 是 LinkInfo。由于UserData 是一个很大的表，通常会被存放到 HDFS 文件中，Spark 系统会根据每个 RDD 元素的数据来源，把每个 RDD 元素放在相应的节点上。比如，从工作节点 上的 HDFS 文件块 (block)中读取到的记录，其生成的 RDD 元素 ((UserID，UserInfo)形式的键值对 ，就会被放在节点上，从节点上的 HDFS 文件块 (block)中读取到的记录，其生成的 RDD 元素会被放在节点上，最终userData 这个 RDD 的元素就会分布在节点u1、u2.....um上。
然后，执行连接操作 userData,join(events)得到连接结果。如图  所示，在默认情况下，连接操作会将两个数据集中的所有的 key 的哈希值都求出来，将哈希值相同的记录传送到同一台机器上之后在该机器上对所有 key 相同的记录进行连接操作。比如，对于 userData 这个 RDD 而言，它在节点山上的所有 RDD 元素，都需要根据 key 的值进行哈，然后，根据哈希值再分发到 j1、j2.....j这些节点上;在节点u上的所有 RDD 元素，也需要根据 key 的值进行哈希，然后，根据哈希值再分发到j1……jk这些节点上;同理，u1……um等节点上的 RDD元素，都需要进行同样的操作对于events 这个RDD 而言，也需要执行同样的操作。可以看出，在这种情况下，每次进行连接操作都会有数据混洗的问题，造成了很大的网络传输开销。

![1679321456313](/images/spark/42.png)

实际上，由于userData 这个 RDD 要比 events 大很多，所以，可以选择对 userData 进行分区。比如.可以采用哈希分区方法，把 userData 这个 RDD 分区成 m 个分区，这些分区分布在节点 、u...“u，上。对userData 进行分区以后，在执行连接操作时，就不会产生图 4-10 中的数据混洗情况。如图所示，由于已经对 userData 根据哈希值进行了分区，因此，在执行连接操作时，不需要再把 userData中的每个元素进行哈希求值以后再分发到其他节点上，只需要对 events 这个 RDD 的每个元素求哈希值(采用与 userData 相同的哈希函数)。然后，根据哈希值把每个 events 中的 RDD 元素分发到对应的节点u、u····um上面。整个过程中，只有 events 发生了数据混洗，产生了网络通信，而 userData的数据都是在本地引用，不会产生网络传输开销。由此可以看出，Spark 通过数据分区，可以大大降低一些特定类型的操作(比如join()、leftOuterJoin()、groupByKey()、reduceByKey()等)的网络传输开销。

![1679321697993](/images/spark/43.png)

#### 2.分区的原则

![1679322183409](/images/spark/44.png)

RDD分区的一个原则是使得分区的个数尽量等于集群中的 CPU核心(Core)数目。对于不同的Spark 部署模式 (Local 模式、Standalone 模式、YARN 模式、Mesos 模式)而言，都可以通过设置spark.defaultparallelism 这个参数的值，来配置默认的分区数目。一般而言，各种模式下的默认分区数目如下。
Local模式:默认为本地机器的 CPU 数目，若设置了 local[N]，则默认为 N。Standalone 或YARN模式:在“集群中所有 CPU 核心数目总和”和“2”这二者中取较大值作为
默认值。
Mesos 模式:默认的分区数为 8。

#### 3.设置分区的个数

可以手动设置分区的数量，主要包括两种方式: 创建 RDD 时手动指定分区个数;使用repartition方法重新设置分区个数。

##### (1)创建RDD 时手动指定分区个数

![1679322207468](/images/spark/45.png)

在调用 textFile()和 parallelize()方法的时候手动指定分区个数即可，语法格式如下:

```
sc.textFile(path, partitionNum)
```

其中，path 参数用于指定要加载的文件的地址，partitionNum 参数用于指定分区个数。下面是个分区的实例。

```
list = [1,2,3,4,5]
rdd = sc.parallelize(list,2) //设置两个分区
```

对于 parallelize()而言，如果没有在方法中指定分区数，则默认为 spark.default,parallelism。对于textFile()而言，如果没有在方法中指定分区数，则默认为 min(defaultParallelism,2)，其中defaultParallelism 对应的就是 spark.default,parallelism。如果是从HDFS 中读取文件，则分区数为文件分片数(比如，128MB/片 )。

##### (2)使用repartition 方法重新设置分区个数

![1679322237913](/images/spark/46.png)

通过转换操作得到新 RDD 时，直接调用 repartition 方法即可。例如:

```
data = sc.parallelize([1,2,3,4,5],2)
len(data.glom().collect ()) #显示 data 这个 RDD 的分区数量
2
rdd = data.repartition(1) #对 data 这个RDD进行重新分区
len(rdd.glom().collect())#显示 rdd 这个 RDD的分区数量
1
```

#### 4.自定义分区方法

![1679322265099](/images/spark/47.png)

![1679322297300](/images/spark/48.png)

Spark 提供了自带的 HashPartitioner (哈希分区)与 RangePartitioner ( 区城分区)，能够满足大数应用场景的需求。与此同时，Spark 也支持自定义分区方式，即通过提供一个自定义的分区函数来控制 RDD 的分区方式，从而利用领域知识进一步减少通信开销。需要注意的是，Spark 的分区函数针对的是(key;value)类型的 RDD，也就是说，RDD 中的每个元素都是(key,value)类型，然后，分区数根据 key对RDD 元素进行分区。因此，当需要对一些非(key,value)类型的 RDD进行自定义分区时需要首先把 RDD 元素转换为(key,value)类型，然后再使用分区函数。
下面是一个实例，要求根据 key 值的最后一位数字将 key 写入到不同的文件中，比如，10 写入到part-00000，11写入到 part-00001，12 写入到 part-00002。打开一个 Linux 终端，使用 vim 编辑器创建一个代码文件“/usr/local/spark/mycode/rdd/TestPartitioner.py”，输入以下代码:

![1679322332812](/images/spark/49.png)

```python
from pyspark import SparkConf, SparkContext
def MyPartitioner(key):
	print("MyPartitioner is running")
	print('The key is %d' % key)
	return key%10
def main():
	print("The main function is running")	
	conf=SparkConf().setMaster("local").setAppName("MyApp")
	sc=SparkContext(conf=conf)
	data=sc.parallelize(range(10),5)
	data.map(lambda x:(x,1)) \
	         .partitionBy(10,MyPartitioner)\
             .map(lambda x:x[0]) \
	         .saveAsTextFile("File:///usr/local/spark/mycode/add/partitioner")
if __name__=='__main__':
	main()



```

出现问题

```ModuleNotFoundError: No module named 'pyspark'```

```
apt install python3-pip
pip install pyspark
```

在上述代码中，data=sc.parallelize(range(10),5)这行代码执行后，会生成一个名称为 data 的 RDD这个RDD 中包含了 0、1、2、3.....9 共 10 个整型元素，并被分成 5个分区。data,map(lambda x:(x,1)表示把 data 中的每个整型元素取出来，转换成(key,value)类型。比如，把1 这个元素取出来以后转按成(1,1)，把2这个元素取出来以后转换成(2,1)，这是因为，自定义分区函数要求 RDD 元素的类型必须是(key, value)类型。partitionBy(10,MyPartitioner表示调用自定义分区函数，把(0,1)、(,)、(2,1)、(3,1)....(9,1)这些 RDD 元素根据尾号分成10个分区。划分分区完成以后,再使用 map(ambda x:x[0).把(0,1)、(1,1)、(2,1)、(3,1) .·....(9,1)等(key,value)类型元素的 key 提取出来，得到 0、1、2、3...9.最后调用 saveAsTextFile0方法把 RDD的 10个整型元素写入到本地文件中。

使用如下命令运行 TestPartitioner.py:

```
cd /usr/local/spark/mycode/rdd
python3 TestPartitioner.py
```

or

```
$ cd /usr/local/spark/mycode/rdd
S /usr/local/spark/bin/spark-submit TestPartitioner.py
```

程序运行后会返回如下信息:

```
The main function is running
MyPartitioner is running
The key is 0
MyPartitioner is running
The key is 1
MyPartitioner is running
The key is 9
```



![1679322360532](/images/spark/50.png)

运行结束后可以看到，在本地文件系统的“file:///usr/local/spark/mycode/rdd/partitioner”目录下面，会生成 part-00000、part-00001、part-00002.....part-00009 和_SUCCESS 等文件。其中,part-00000文件中包含了数字 0，part-00001 文件中包含了数字 1，part-00002 文件中包含了数字 2

### 综合实例

假设有一个本地文件“/usr/local/spark/mycode/rdd/word.txt”，里面包含了很多行文本，每行文本由多个单词构成，单词之间用空格分隔。可以使用如下语句对 word.txt 中的单词进行词频统计(即统计每个单词出现的次数 ):

![1679329145076](/images/spark/51.png)

```python
lines=sc.textFile("file:///usr/local/spark/mycode/rdd/word.txt")
```

```python
wordCount=lines.flatMap(lambda line:line.split(" ")).map(lambda word:(word,1)).reduceByKey(lambda a,b:a+b)
```

```python
print(wordCount.collect())
```

![1679329176974](/images/spark/52.png)

![1679329211688](/images/spark/53.png)

![1679329237135](/images/spark/54.png)

在实际应用中，单词文件可能非常大，会被保存到分布式文件系统 HDFS 中，Spark 和 Hadoop会统一部署在一个集群上。如图所示，HDFS 的名称节点(HDFS NN)和 Spark 的主节点( SparkMaster)可以分开部署，而HDFS 的数据节点(HDFS DN)和 Spark 的从节点 ( Spark Worker)会部署在一起。这时采用 Spark 进行分布式处理,可以大大提高词频统计程序的执行效率,这是因为,SparkWorker 可以就近处理与自己部署在一起的 HDFS 数据节点中的数据。

对于词频统计程序 WordCount 而言，该程序分布式运行在每个 Slave 节点的每个分区上，统计本分区内的单词计数，然后将它传回给 Driver，再由 Driver 合并来自各个分区的所有单词计数，形成最终的单词计数。