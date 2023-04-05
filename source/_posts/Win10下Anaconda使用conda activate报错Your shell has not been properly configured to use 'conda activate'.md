---
title: anaconda安装其他东西
tag: anaconda
categories: 2023
---

#  Win10下Anaconda使用conda activate报错Your shell has not been properly configured to use 'conda activate'

![1677212390344](/images/anaconda/1.png)
<!-- more -->
```
PS E:\dijango> conda activate base

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
If using 'conda activate' from a batch script, change your
invocation to 'CALL conda.bat activate'.
To initialize your shell, run
    $ conda init <SHELL_NAME>

Currently supported shells are:

- bash
- cmd.exe
- fish
- tcsh
- xonsh
- zsh
- powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.


```

先以管理员身份打开cmd。
试一下conda activate 环境名称。
如果命令行提示

```
Your shell has not been properly configured to use ‘conda activate’.
```


然后下面还提示

`conda init <SHELL NAME>`

就按照他的要求，输入一下

`conda init cmd.exe`

或者

`conda init powershell`

这两条都试试，回车，重启cmd说不定就好了。反正我好了。
![1677212587799](/images/anaconda/2.png)

