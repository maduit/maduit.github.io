---
title: 解决bash hexo command not found
tag: hexo
categories: 2023
---
# 解决bash: hexo: command not found

检查 nodejs 和 npm 是否正常，依次输入命令 `node -v` 和 `npm -v` 看看是否有相关版本信息

<!-- more -->
![1669786706325](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/1.png)

出现了版本信息就证明 nodejs 和 npm 是没有问题的，那么就应该是环境变量的配置问题了，在【此电脑】右键【属性】，依次选择【高级系统设置】-【环境变量】，选择系统变量 Path，将 `node_modules` 下的 `.bin` 文件路径添加到 Path 里面

注意你的博客目录下应该有两个 `node_modules` 文件夹

F:\blog 和F:\blog\BLOG

![1669786987745](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/2.png)

![1669787025987](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/3.png)

我是加了第一个module在环境变量

![1669787251679](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/4.png)

别的博主有的加了第二个module

总之自己试一下按哪个

环境变量添加好了之后重新打开 git 即可运行 hexo 命令，如果此时仍然无法执行 hexo 命令，那就只能拿出终极绝招了，运行命令 `npm install hexo-cli -g` 重新安装 hexo 即可！