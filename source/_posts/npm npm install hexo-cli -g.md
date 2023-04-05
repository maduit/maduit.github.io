---
title: npm install 包，没有报错，提示安装成功，但是项目中没有出现node_modules也没有安装的包
tag: hexo
categories: 2023
---

# npm install 包，没有报错，提示安装成功，但是项目中没有出现node_modules也没有安装的包

```
npm install hexo-cli -g
```
<!-- more -->
在当前文件夹目录下npm安装 hexo-cli -g

在当前文件夹不显示

因为这是全局安装

所以会跑到node.js文件夹目录底下

![1669790121108](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/5.png)

去这边找会发现一个hexo-cli的文件夹

点击这个博客[https://blog.csdn.net/qq_38613992/article/details/103769192]

**查看npm的配置**

```javascript
npm config list
```

一开始我是没有global=？？？ 的

![npm配置信息](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/6.png)

**2.查看全局下，是否有自己安装的包**

```javascript
npm root -g//获取到全局安装目录


```

一般看上面那个F盘的图，会在那里

3.修改npm配置信息，查看 图例1，global属性是否安装到全局，如果你的这里是true，那么，就算你安装一个包时，没有写-g，它也会自动将你的包安装到全局！ — 修改配置信息 方法一：命令行输入

```
npm config set global=false，

npm config set global=false
```

再次查看配置，确认是否修改

```javascript
npm config list
```


![1669790489536](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/7.png)

然后再

```
npm install hexo-cli
```

后面不要-g就会在自己的文件夹里出现node_moudle了

你可以试试有-g的```npm install hexo-cli``，这样的话，就又会跑到node.js的global的moudle的文件夹里了



安装完成，如图

![1669790709401](/images/hexo%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3/8.png)