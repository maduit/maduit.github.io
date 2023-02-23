---
title: maven 找不到依赖项 javax.servlet:servlet-api:${servlet-api.version}
tag: maven
categories: 2023
---

# 找不到依赖项 javax.servlet:servlet-api:${servlet-api.version}

1.下载maven百度
<!-- more -->
2.maven仓库查找

https://mvnrepository.com/

直接搜索

比如
![1672900140564](/images/maven/1.png)

直接搜索

![1672900240366](/images/maven/2.png)

然后

![1672900269701](/images/maven/3.png)

点击之后往下移动

找到这一行

![1672900285401](C/images/maven/4.png)

贴到pom.xml文件里

![1672983178197](/images/maven/5.png)



坐标组成

![1672983299156](/images/maven/6.png)

配置本地仓库

![1672987574149](/images/maven/7.png)

![1672987601115](/images/maven/8.png)

中央仓库

![1672988046524](/images/maven/9.png)

id唯一标识符，用来区分不同的mirror元素

mirrorOf代替哪个仓库

url镜像的URL