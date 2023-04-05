---
title: Import pymysql could not be resolved from
tag: django
categories: 2023
---


![1677241477982](/images/anaconda/3.png)

Import "pymysql" could not be resolved from 
<!-- more -->
![1677241534025](/images/anaconda/4.png)

在anaconda里面装一个

首先先切进django的虚拟环境里面

  在Anaconda中，可以通过使用conda命令来创建和管理虚拟环境。要切换已创建的虚拟环境，可以使用以下命令：

1. 列出已有环境：

```
conda info --envs
```




2. 激活目标环境：

```
conda activate <env_name>
```



这里`<env_name>`是你想要激活的虚拟环境的名称。激活环境后，你可以在该环境中使用安装的软件包和工具。

3. 取消激活当前环境：

```
conda deactivate
```



这将使当前环境不再处于活动状态，回到默认的基础环境。 

注意：如果你在使用Anaconda Navigator，也可以通过选择“Environments”选项卡，然后单击目标环境的名称来激活虚拟环境。  

`conda install pymysql`

看你anaconda用的是什么源，国内的记得关闭魔法上网工具