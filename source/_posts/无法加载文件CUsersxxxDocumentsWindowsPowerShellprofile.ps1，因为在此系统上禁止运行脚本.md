---
title: 无法加载文件C:\Users\xxx\Documents\WindowsPowerShell\profile.ps1
tag: powershell
categories: 2023
---
# 无法加载文件C:\Users\xxx\Documents\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本
<!-- more -->
## 问题描述

打开 PowerShell 提示如下报错信息。

```plainText
Windows PowerShell
版权所有（C） Microsoft Corporation。保留所有权利。

安装最新的 PowerShell，了解新功能和改进！https://aka.ms/PSWindows

. : 无法加载文件 C:\Users\87897\Documents\WindowsPowerShell\profile.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参
阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 3
+ . 'C:\Users\87897\Documents\WindowsPowerShell\profile.ps1'
+   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
1234567891011
```

## 原因分析

输入 `get-ExecutionPolicy` 输出 `Restricted`，即脚本执行策略受限。

```plainText
PS C:\Windows\system32> get-ExecutionPolicy
Restricted
12
```

## 解决方案

更换脚本执行策略：`set-ExecutionPolicy RemoteSigned`，然后输入 `Y`。

```plainText
PS C:\Windows\system32> set-ExecutionPolicy RemoteSigned

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 https:/go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [A] 全是(A)  [N] 否(N)  [L] 全否(L)  [S] 暂停(S)  [?] 帮助 (默认值为“N”): Y
123456
```

更换完成后，再次使用命令 `get-ExecutionPolicy` 查看脚本执行策略。

```plainText
PS C:\Windows\system32> get-ExecutionPolicy
RemoteSigned
12
```

可以发现已经更改了。问题完美解决，over~~😊