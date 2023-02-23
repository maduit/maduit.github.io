---
title: C++文件操作之freopen
tag: c++
categories: 2023
---
# C++文件操作之freopen
<!-- more -->
```
freopen("xxx.in","r",stdin);	//输入文件
freopen("xxx.out","w",stdout);	//输出文件
```



```
#include<cstdio>
#include<iostream>
using namespace std;
int main()
{
	freopen("a+b.in","r",stdin);
	freopen("a+b.out","w",stdout);
	//以上是模板
	int a,b;
	cin>>a>>b;
	cout<<a+b<<endl;
	return 0;
}

```

