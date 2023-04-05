---
title: 如何在Linux系统下编译C++
tag: linux_c++
categories: 2023
---



# 如何在Linux系统下编译C++

### 方法一:

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int main(){

    cout<<"hello_word";
    return 0;
}
```

![1679061064013](/images/NOI/1.png)

编译

```
gcc 1.cpp -lstdc++ 
```

生a.out文件

![1679061165889](/images/NOI/2.png)

运行a.out

```
./a.out
```

![1679061280444](/images/NOI/3.png)

### 方法二:

```c++
#include<iostream>
#include<cstdio>
using namespace std;
int main(){

    cout<<"hello_word222";
    return 0;
}
```

![1679061782953](/images/NOI/5.png)

编译

```
g++ 2.cpp -o 2output
```

运行2output

```
./2output
```

