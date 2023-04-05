---
title:  DataError at /index/add_member 1366, "Incorrect string value :\\xE4\\xBC\\x98\\xE
tag: mysql
categories: 2023
---

825577940@qq.com DataError at /index/add_member 1366, "Incorrect string value: '\\xE4\\xBC\\x98\\xE

# ERROR 1366 (HY000):Incorrect string value解决方案
<!-- more -->
https://zhuanlan.zhihu.com/p/53941345

![1677985811006](/images/mysql/1.png)

![1677985852047](/images/mysql/2.png)

after

![1677985952688](/images/mysql/3.png)

before

![1677985979541](/images/mysql/4.png)

然后把数据库删除，重新创建数据库，再导入数据库文件，就又是一条好汉



![1677988728653](/images/mysql/5.png)

![1677988745809](/images/mysql/6.png)