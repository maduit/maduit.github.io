---
title: Python类
tag: python
categories: 2023
---

# Python类

### 类(class) 和 对象(object)

类：创建对象的模板，定义对象将会拥有的属性和函数

__init__函数：每个类必须定义的函数，对象创建语句时自动执行

```python
class myday: #建立一个类模板

     def __init__():

          .....



day1 = myday()      #创建一个myday类的对象 
```

* python类
  类名：mayday
  属性：name和emotion
  函数：**init**函数，wake函数，eat函数

```python
# 类(class)
class  myday:
        def __init__(self):             #每个类必须定义(def)    __init__函数，每个函数第一参数都是self
               self.name = "Xiao Ming"  #myday对象拥有属性name和emotion
               self.emotion = "happy"

        def wake(self,event):
               if event == "上课":
                     self.emotion = "still happy"

        def eat(self, food):
               if food == "牛肉":
                     self.emotion = "more happy"
        #def __eq__(self,other):
            #return self.name == other.name
day2 = myday()
#创建对象时会自动执行__init__方法
print(day2.emotion)
print(day2.name)

```

```__init__函数添加参数  ```

``` 创建对象时传入self之后的参数```

```python
class  myday:
        def __init__(self,name,emotion):                                                                 
            self.emotion = emotion  #定义对象属性“self.emotion”，将这个属性赋值为函数传入的参数“emotion”
            self.name = name
        def wake(self,event):
            if event == "上课":
                self.emotion = "still happy"
    
        def eat(self, food):
            if food == "牛肉":
                self.emotion = "more happy"
        #def __eq__(self,other):
            #return self.name == other.name
day2 = myday("me","very happy")
#创建对象时会自动执行__init__函数
print(day2.name)
print(day2.emotion)


```

```python
class  myday:
        def __init__(self,name,emotion):                                                                 
            self.emotion = emotion  #定义对象属性“self.emotion”，将这个属性赋值为函数传入的参数“emotion”
            self.name = name
        def wake(self,event):
            if event == "上课":
                self.emotion = "still happy"
    
        def eat(self, food):
            if food == "牛肉":
                self.emotion = "more happy"
        #def __eq__(self,other):
            #return self.name == other.name
day2 = myday("me","very happy")
#调用对象函数时，从self之后的参数开始传入
day2.eat("jiaozi")
print(day2.emotion)
day2.eat("牛肉")
print(day2.emotion)


    

```



### 任务：编写一个类继承myday类

```python
class  myday:
        def __init__(self,name,emotion):                                                                 
            self.emotion = emotion  #定义对象属性“self.emotion”，将这个属性赋值为函数传入的参数“emotion”
            self.name = name
        def wake(self,event):
            if event == "上课":
                self.emotion = "still happy"
    
        def eat(self, food):
            if food == "牛肉":
                self.emotion = "more happy"
        #def __eq__(self,other):
            #return self.name == other.name
day2 = myday("me","very happy")

#创建对象时，程序会自动执行__init__函数
day2.eat("jiaozi")
print(day2.emotion)
day2.eat("牛肉")
print(day2.emotion)

#类的继承     和     函数重写
class night(myday): #night类继承myday类
    	def __init__(self):
       		 self.emotion = "nice"
       		 self.name = "me"
 	   def play(self):
       		 self.emotion = "so nice"
night1 = night()
night1.eat("羊肉")  #night继承myday类除__init__之外的函数
print(night1.emotion)
night1.eat("牛肉")
print(night1.emotion)

```

###  **一个类是否等于另一个类？**



```python
day1 = myday("a","happy")
day2 = myday("a","happy")

day1.name == day2.name   #True or False
day1 == day2    #True or False?
#（如果我们在类里定义一个__eq__函数，当我们执行 day1 == day2语句的时候，程序会自动执行__eq__函数）

```

![1679290854192](/images/大数据可视化/1.png)





### **多线程开发案例**





````python
import time
import threading

class TestThread(threading.Thread):
    def __init__(self, para='hi', sleep=3):
         super().__init__()
         self.para = para
         self.sleep = sleep
    def run(self):
         """线程内容"""
         time.sleep(self.sleep)
         print(self.para)

thread_hi = TestThread()
thread_hello = TestThread('hello', 1)
 # 启动线程
thread_hi.start()
thread_hello.start()



````

### 任务！：按照如下描述定义一个类，代表一个企业

class company: init函数：定义资金（money)属性，产品(product)属性和价格（price）属性

register函数：公司注册，修改资金属性

produce函数：制造，修改资金属性，修改产品属性

sale函数：销售，修改资金属性和产品属性

research函数：研发，修改资金属性和价格属性

* 任务！：定义另一个类，继承company类，重写research函数（子类需要添加至少一个新的函数，并重写至少一个父类的函数。）

```python
class company:
    def __init__(self,money,product,price) :
        self.money=money
        self.product=product
        self.price=price
    def register(self,tol_money):#注册资金
        self.money=tol_money
    def produce(self,pro_money,product_num):
        self.money-=pro_money#生产就减少了
        self.product+=product_num#产品多了
    def sale(self,sal_money,product_num):
        self.money+=sal_money#钱多了
        self.product-=product_num#产品减少了
    def research(self,re_money,re_price):
        self.money-=re_money#钱减少了
        self.price+=re_price#价格上去了
company1=company("1000","机器","100/2")
company1.register("5000")
print(company1.money)
```

research函数：研发，修改资金属性和价格属性，产品属性

invert函数:投资，修改金钱属性

```python
class BigCompany(company):
    def __init__(self,money,product,price):
        super().__init__(money,product,price)

    def research(self,re_money,re_price,re_prodcut):
        self.money=re_money#钱减少了
        self.price+=re_price#价格上去了
        self.product+=re_prodcut#产品多了
    def inverst(self,invstment_money):
        self.money-=invstment_money

company2=BigCompany(7000,2,50)
company2.research(8000,80,90)
company2.inverst(908070)
print(company2.money)
        
```

