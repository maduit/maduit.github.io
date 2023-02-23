---
title: PyQt5
tag: PyQt5
categories: 2022
---
## 安装PyQt5

打开Anaconda Promt，切换到对应环境输入：
<!-- more -->
 ```
pip install PyQt5 -i https://pypi.douban.com/simple
 ```

## 安装PyQt5-tools

 ```
pip install PyQt5-tools -i https://pypi.douban.com/simple

 ```

打开

Anaconda prompt

![1669202006491](./images/pyqt5/1.png)

输入:

```
PyQt5-tools designer
```

它有很多东西

![1669202116133](./images/pyqt5/2.png)

```
  designer
  installuic
  qmlscene
  qmltestrunner
```

不知道其他是啥（待解决）





--------------------

![1669202186551](./images/pyqt5/3.png)

直接点击创建（main window）

![1669202245388](./images/pyqt5/4.png)

随便拖两个button上去

![1669202300112](./images/pyqt5/21.png)

另存为

![1669202348711](./images/pyqt5/5.png)

然后回到anaconda那个窗口，

我们要把这个.ui文件转化为py文件

方法一：

```
python -m PyQt5.uic.pyuic test.ui -o test.py
```

![1669202702551](./images/pyqt5/6.png)

要切换到相对应的文件夹路径下再运行

![1669202755853](./images/pyqt5/7.png)

已经生成了

方法二：

太烦了，不写

F:\anaconda\EMPYTY\pkgs\pyqt-5.9.2-py39hd77b12b_6\Library\bin

大概在这个路径里面

![1669203475844](./images/pyqt5/7.1.png)

![1669203905200](./images/pyqt5/7.2.png)

![1669203924735](./images/pyqt5/7.3.png)

 很鸡肋，虽然写的前面的那玩意少了点，但是要吧ui移到当前文件夹里面，辣鸡

```
pyuic5 test.ui -o test.py
```



方法三：
直接使用扩展程序打开

pycharm扩展程序（vscode没找到在哪）

![1669203187056](./images/pyqt5/8.png)

不太好搞（不想写，没看懂）

综上，用方法一

1.水平布局

![1669204341946](./images/pyqt5/9.png)

![1669204535028](./images/pyqt5/10.png)

回到vscode里面

![1669204935069](./images/pyqt5/11.png)

![1669205119299](./images/pyqt5/12.png)

```python
import sys
import shuiping
from PyQt5.QtWidgets import QApplication,QMainWindow

#创建QApplication类的实例
app=QApplication(sys.argv)
#创建一个窗口
mainWindow=QMainWindow()
#向主窗口添加控件
ui=shuiping.Ui_MainWindow()
ui.setupUi(mainWindow)
# 显示窗口
mainWindow.show()
# 进入程序的主循环、并通过exit函数确保主循环安全结束
sys.exit(app.exec_())
```

表单布局

![1669206548415](./images/pyqt5/13.png)

垂直布局

![1669206597151](./images/pyqt5/14.png)

栅格布局

![1669206640854](./images/pyqt5/15.png)



尺寸策略

![1669210040330](./images/pyqt5/16.png)

伙伴关系

![1669210466222](./images/pyqt5/17.png)

tab顺序

![1669210672111](./images/pyqt5/18.png)

![1669210689346](./images/pyqt5/19.png)

编辑信号和槽

![1669211232251](./images/pyqt5/22.png)

![1669211749552](./images/pyqt5/20.png)