---
title: No module named 'pyspark'
tag: spark
categories: 2023
---

# ModuleNotFoundError: No module named 'pyspark'

root@gu-virtual-machine:/usr/local/spark/mycode/remdup# python3 remdup.py
Traceback (most recent call last):
File "/usr/local/spark/mycode/remdup/remdup.py", line 1, in <module>
from pyspark import SparkContext
ModuleNotFoundError: No module named 'pyspark'

1.找到.bashrc文件在哪

```
/home/hadoop
```

编辑环境变量

```
export PYSPARK_HOME=/usr/local/spark
export PYTHONPATH=$PYSPARK_HOME/python:$PYTHONPATH
export PYTHONPATH=$PYSPARK_HOME/python/lib/py4j-0.10.9.5-src.zip:$PYTHONPATH
```

其中```py4j-0.10.9.5-src.zip``

需要在```/usr/local/spark/python/lib/```中自己找自己的是啥版本

然后

``` source .bashrc```

```chatgpt 的prompt```

```怎么将PySpark安装目录添加到PYTHONPATH环境变量中```

![1679936834308](/images/spark/77.png)

https://spark.apache.org/docs/latest/api/python/getting_started/install.html