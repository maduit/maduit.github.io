---
title: UML类图 & UML时序图
tag: 设计模式
categories: 2023
---

## 2.1 UML图

## 2.1 UML类图 & UML时序图

统一建模语言（Unified Modeling Language，UML）是用来设计软件的可视化建模语言。它的特点是简单、统一、图形化、能表达软件设计中的动态与静态信息。UML是一种为面向对象系统的产品进行说明、可视化和编制文档的标准语言，独立于任何一种具体的程序设计语言。

1997 年 UML 被国际对象管理组织（OMG）采纳为面向对象的建模语言的国际标准。它的特点是简单、统一、图形化、能表达软件设计中的动态与静态信息。

**应用场景**

UML 能为软件开发的所有阶段提供模型化和可视化支持。而且融入了软件工程领域的新思想、新方法和新技术，使软件设计人员沟通更简明，进一步缩短了设计时间，减少开发成本。

UML 具有很宽的应用领域。其中最常用的是建立软件系统的模型，但它同样可以用于描述非软件领域的系统，如机械系统、企业机构或业务过程，以及处理复杂数据的信息系统、具有实时要求的工业系统或工业过程等。总之，UML 可以对任何具有静态结构和动态行为的系统进行建模，而且使用于从需求规格描述直至系统完成后的测试和维护等系统开发的各个阶段。

UML 模型大多以图表的方式表现出来，一份典型的建模图表通常包含几个块或框、连接线和作为模型附加信息的文本。这些虽简单却非常重要，在 UML 规则中相互联系和扩展。

语言是包括文字和图形的，有很多内容文字是无法表达的。比如建筑设计图纸吗，里面存在多图形，光用文字并不能表达清楚建筑设计。在建筑界，有一套标准来描述设计，同样道理，在软件开发界，也需要一套标准来帮助我们做好软件开发的工作。UML 就是其中的一种标准，注意这可不是唯一标准，只是 UML 是大家比较推崇的一种标准而已。UML 并不是强制性标准，没有规定在软件开发中一定要用 UML，但是我们需要包括 UML 在内的各种标准，来提高软件开发的水平。

**基本构件**

UML 建模的核心是模型，模型是现实的简化、真实系统的抽象。UML 提供了系统的设计蓝图。当给软件系统建模时，需要采用通用的符号语言，这种描述模型所使用的语言被称为建模语言。在 UML 中，所有的描述由事物、关系和图这些构件组成。下图完整地描述了所有构件的关系。

![img](https://p.ananas.chaoxing.com/star3/origin/8b41ffb818f6758e08c3a4307c24d76f.png)

**事物**

事物是抽象化的最终结果，分为结构事物、行为事物、分组事物和注释事物。

1. 结构事物

结构事物是模型中的静态部分，用以呈现概念或实体的表现元素，如下表所示。

![img](https://p.ananas.chaoxing.com/star3/origin/6d050090c2dd17905c284bec11c2ded1.png)

2. 行为事物

行为事物指 UML 模型中的动态部分，如下表所示。

![img](https://p.ananas.chaoxing.com/star3/origin/a1b45aed6c2bc4351ca486f51f349ff1.png)

3. 分组事物

目前只有一种分组事物，即包。包纯碎是概念上的，只存在于开发阶段，结构事物、行为事物甚至分组事物都有可能放在一个包中，如下表所示。

![img](https://p.ananas.chaoxing.com/star3/origin/45f2e9d2f5a63f6a015e119e83ad678e.png)

4. 注释事物

注释事物是解释 UML 模型元素的部分，如下表所示。

![img](https://p.ananas.chaoxing.com/star3/origin/c01bdb3ec33250922bf8bd337031ad30.png)

UML 从目标系统的不同角度出发，UML2.0 一共有 13 种图（UML1.5 定义了 9 种，UML2.0 增加了 4 种），别是类图、对象图、构件图、部署图、活动图、状态图、用例图、时序图、协作图 9 种，以及包图、组合结构图、时间图、交互概览图 4 种。

![img](https://p.ananas.chaoxing.com/star3/origin/1b62f09be7dfc3a727620cf046e29d5b.png)

在 UML 2.0 的 13 种图中，类图（Class Diagrams）是使用频率最高的 UML 图之一。类图描述系统中的类，以及各个类之间的关系的静态视图，能够让我们在正确编写代码之前对系统有一个全面的认识。类图是一种模型类型，确切地说，是一种静态模型类型。类图表示类、接口和它们之间的协作关系，用于系统设计阶段。

### 2.1.1 类图概述 

类图(Class diagram)是显示了模型的静态结构，特别是模型中存在的类、类的内部结构以及它们与其他类的关系等。类图不显示暂时性的信息。类图是面向对象建模的主要组成部分。

### 2.1.2 类图作用 

- 在软件工程中，类图是一种静态的结构图，描述了系统的类的集合，类的属性和类之间的关系，可以简化了人们对系统的理解；
- 类图是系统分析和设计阶段的重要产物，是系统编码和测试的重要模型。

### 2.1.3 类图表示

   **1.类**

​    类（Class）是指具有相同属性、方法和关系的对象的抽象，它封装了数据和行为，是面向对象程序设计（OOP）的基础，具有封装性、继承性和多态性等三大特性。

  (1) 类名（Name）是一个字符串，例如，Student。

  (2) 属性（Attribute）是指类的特性，即类的成员变量。

  (3) 操作（Operations）是类的任意一个实例对象都可以使用的行为，是类的成员方法。    在UML类图中，类使用包含类名、属性(field) 和方法(method) 且带有分割线的矩形来表示，比如下图表示一个Employee类，它包含name,age和address这3个属性，以及work()方法。

![Employee.jpg](https://p.ananas.chaoxing.com/star3/origin/5de8b469efc781f32fd20dddb5633f6e.jpg)

​    属性/方法名称前加的加号和减号表示了这个属性/方法的可见性，UML类图中表示可见性的符号有三种：

- +：表示public
- -：表示private
- \#：表示protected

​    属性的完整表示方式是： **可见性 名称 ：类型 [ = 缺省值]**

​    方法的完整表示方式是： **可见性 名称(参数列表) [ ： 返回类型]**

> 注意：
>
> 1，中括号中的内容表示是可选的
>
> 2，也有将类型放在变量名前面，返回值类型放在方法名前面

**举个例子：**

![demo.png](https://p.ananas.chaoxing.com/star3/origin/d4c5ec49b8ee9f5ae4c5dccd902290a8.png)

​    上图Demo类定义了三个方法：

- method()方法：修饰符为public，没有参数，没有返回值。
- method1()方法：修饰符为private，没有参数，返回值类型为String。
- method2()方法：修饰符为protected，接收两个参数，第一个参数类型为int，第二个参数类型为String，返回值类型是int。

类图中，需注意以下几点：

- 抽象类或抽象方法用斜体表示
- 如果是接口，则在类名上方加 <<Interface>>
- 字段和方法返回值的数据类型非必需
- 静态类或静态方法加下划线

**另外一个例子：**

请看以下这个类图，类之间的关系是我们需要关注的：

**![_images/uml_class_struct.jpg](https://design-patterns.readthedocs.io/zh_CN/latest/_images/uml_class_struct.jpg)**



- 车的类图结构为<<abstract>>，表示车是一个抽象类；
- 它有两个继承类：小汽车和自行车；它们之间的关系为实现关系，使用带空心箭头的虚线表示；
- 小汽车为与SUV之间也是继承关系，它们之间的关系为泛化关系，使用带空心箭头的实线表示；
- 小汽车与发动机之间是组合关系，使用带实心箭头的实线表示；
- 学生与班级之间是聚合关系，使用带空心箭头的实线表示；
- 学生与身份证之间为关联关系，使用一根实线表示；
- 学生上学需要用到自行车，与自行车是一种依赖关系，使用带箭头的虚线表示；

**2. 接口**

接口（Interface）是一种特殊的类，它具有类的结构但不可被实例化，只可以被子类实现。它包含抽象操作，但不包含属性。它描述了类或组件对外可见的动作。在 UML 中，接口使用一个带有名称的小圆圈来进行表示。

如下所示是图形类接口的 UML 表示。

![img](https://p.ananas.chaoxing.com/star3/origin/64a5bf7166d365b206bdefeb4a66fa20.png)



**3. 类图**

类图（ClassDiagram）是用来显示系统中的类、接口、协作以及它们之间的静态结构和关系的一种静态模型。它主要用于描述软件系统的结构化设计，帮助人们简化对软件系统的理解，它是系统分析与设计阶段的重要产物，也是系统编码与测试的重要模型依据。

类图中的类可以通过某种编程语言直接实现。类图在软件系统开发的整个生命周期都是有效的，它是面向对象系统的建模中最常见的图。如下所示是“计算长方形和圆形的周长与面积”的类图，图形接口有计算面积和周长的抽象方法，长方形和圆形实现这两个方法供访问类调用。

**![img](https://p.ananas.chaoxing.com/star3/origin/f95aee4f5136a105551af5aa732544d7.png)**







### 2.1.4 类与类之间关系的表示

1. **关联关系(association)**    

 关联关系是用一条直线表示的；它描述不同类的对象之间的结构关系；它是一种静态关系， 通常与运行状态无关，一般由常识等因素决定的；它一般用来定义对象之间静态的、天然的结构； 所以，关联关系是一种“强关联”的关系；

比如，乘车人和车票之间就是一种关联关系；学生和学校就是一种关联关系；

关联关系默认不强调方向，表示对象间相互知道；如果特别强调方向，如下图，表示A知道B，但 B不知道A；

![uml_association.jpg](https://p.ananas.chaoxing.com/star3/origin/961e252cc29eacbaaa509dbca06e867d.jpg)

注：在最终代码中，关联对象通常是以成员变量的形式实现的；

关联关系是类与类之间最常用的一种关系，分为一般关联关系、聚合关系和组合关系。我们先介绍一般关联。关联又可以分为单向关联，双向关联，自关联。

a）单项关联

![customer_address.png](https://p.ananas.chaoxing.com/star3/origin/4e20737215be9cc0838f06ba6d9e3e73.png)

在UML类图中单向关联用一个带箭头的实线表示。上图表示每个顾客都有一个地址，这通过让Customer类持有一个类型为Address的成员变量类实现。

b）双向关联

![customer_product.png](https://p.ananas.chaoxing.com/star3/origin/0647a820494e08997a6b773174116f72.png)

从上图中我们很容易看出，所谓的双向关联就是双方各自持有对方类型的成员变量。

在UML类图中，双向关联用一个不带箭头的直线表示。上图中在Customer类中维护一个List<Product>，表示一个顾客可以购买多个商品；在Product类中维护一个Customer类型的成员变量表示这个产品被哪个顾客所购买。

c）自关联

![node.png](https://p.ananas.chaoxing.com/star3/origin/645df4c3220d37685e4cdb81c926e465.png)

自关联在UML类图中用一个带有箭头且指向自身的线表示。上图的意思就是Node类包含类型为Node的成员变量，也就是“自己包含自己”。

### 2. 聚合关系(aggregation)

聚合关系是关联关系的一种，是强关联关系，是整体和部分之间的关系。

聚合关系也是通过成员对象来实现的，其中成员对象是整体对象的一部分，但是成员对象可以脱离整体对象而独立存在。例如，学校与老师的关系，学校包含老师，但如果学校停办了，老师依然存在。

聚合关系用一条带空心菱形箭头的直线表示，如下图表示A聚合到B上，或者说B由A组成；

![uml_aggregation.jpg](https://p.ananas.chaoxing.com/star3/origin/29ec48ef1ad32353bbdb2f17e3ad5cee.jpg)

下图所示是大学和教师的关系图：

![image-20191229173422328.png](https://p.ananas.chaoxing.com/star3/origin/72d895954f4087e90cd2c017a8ac65b2.png)

### 3. 组合关系(composition)

组合表示类之间的整体与部分的关系，但它是一种更强烈的聚合关系。

在组合关系中，整体对象可以控制部分对象的生命周期，一旦整体对象不存在，部分对象也将不存在，部分对象不能脱离整体对象而存在。例如，头和嘴的关系，没有了头，嘴也就不存在了。

在 UML 类图中，组合关系用带实心菱形的实线来表示，菱形指向整体。如下图表示A组成B，或者B由A组成；

![uml_composition.jpg](https://p.ananas.chaoxing.com/star3/origin/689c10062581c795135afd579f7c739e.jpg)

下图所示是头和嘴的关系图：

![image-20191229173455149.png](https://p.ananas.chaoxing.com/star3/origin/d85e919e84777cf76b88ff21e034508c.png)

与聚合关系一样，组合关系同样表示整体由部分构成的语义；比如公司由多个部门组成；

但组合关系是一种强依赖的特殊聚合关系，如果整体不存在了，则部分也不存在了；例如， 公司不存在了，部门也将不存在了；

### 4. 依赖关系(dependency)

依赖关系是一种使用关系，它是对象之间耦合度最弱的一种关联方式，是临时性的关联。在代码中，某个类的方法通过局部变量、方法的参数或者对静态方法的调用来访问另一个类（被依赖类）中的某些方法来完成一些职责。

在 UML 类图中，依赖关系使用带箭头的虚线来表示，箭头从使用类指向被依赖的类。如下图表示A依赖于B；他描述一个对象在运行期间会用到另一个对象的关系；

![uml_dependency.jpg](https://p.ananas.chaoxing.com/star3/origin/3af99c1229a43e6eda76e06d40101781.jpg)

下图所示是司机和汽车的关系图，司机驾驶汽车：

![image-20191229173518926.png](https://p.ananas.chaoxing.com/star3/origin/cf4014bd5e1ef9db4dad9802f7bd7cb6.png)

与关联关系不同的是，它是一种临时性的关系，通常在运行期间产生，并且随着运行时的变化； 依赖关系也可能发生变化；

显然，依赖也有方向，双向依赖是一种非常糟糕的结构，我们总是应该保持单向依赖，杜绝双向依赖的产生；

注：在最终代码中，依赖关系体现为类构造方法及类方法的传入参数，箭头的指向为调用关系；依赖关系除了临时知道对方外，还是“使用”对方的方法和属性；

### 5. 继承关系/泛化关系(generalization)

继承关系是对象之间耦合度最大的一种关系，表示一般与特殊的关系，是父类与子类之间的关系，是一种继承关系。类的继承结构表现在UML中为：泛化(generalize)与实现(realize)：

继承关系为 is-a的关系；两个对象之间如果可以用 is-a 来表示，就是继承关系：（..是..)

eg：自行车是车、猫是动物

在 UML 类图中，泛化关系用带空心三角箭头的实线来表示，箭头从子类指向父类。如下图表示（A继承自B）；

![uml_generalization.jpg](https://p.ananas.chaoxing.com/star3/origin/8907c51dba42ffa0482fb71cbe52a51c.jpg)

例如，汽车在现实中有实现，可用汽车定义具体的对象；汽车与SUV之间为泛化关系；

![uml_generalize.jpg](https://p.ananas.chaoxing.com/star3/origin/1922c1289aa576d923c28dd2e369735a.jpg)

再比如，Student 类和 Teacher 类都是 Person 类的子类，其类图如下图所示：

![image-20191229173539838.png](https://p.ananas.chaoxing.com/star3/origin/4a3aa059e7bd0c658b8c7f9c38ceb415.png)

注：最终代码中，泛化关系表现为继承非抽象类；

### 6. 实现关系(realize)

实现关系是接口与实现类之间的关系。在这种关系中，类实现了接口，类中的操作实现了接口中所声明的所有的抽象操作。

在 UML 类图中，实现关系使用带空心三角箭头的虚线来表示，箭头从实现类指向接口。

比如：”车”为一个抽象概念，在现实中并无法直接用来定义对象；只有指明具体的子类(汽车还是自行车)，才 可以用来定义对象（”车”这个类在C++中用抽象类表示，在JAVA中有接口这个概念，更容易理解）

![uml_realize.jpg](https://p.ananas.chaoxing.com/star3/origin/65f15a3616c6085fab91d7a7864e7b93.jpg)

再例如，汽车和船实现了交通工具，其类图如图 9 所示。

![image-20191229173554296.png](https://p.ananas.chaoxing.com/star3/origin/ac2c49a538d0cda03187a91c71c75ba5.png)

注：最终代码中，实现关系表现为继承抽象类；

类关系记忆技巧如下表所示。

![img](https://p.ananas.chaoxing.com/star3/origin/2cd964f35657776fb8770597e5059927.png)

注意：UML 的标准类关系图中，没有实心箭头。有些 Java 编程的 IDE 自带类生成工具可能出现实心箭头，主要目的是降低理解难度。

下面用一个经典案例来加深和巩固对类图的理解。下图是对动物衍生关系描述的类图。这个图非常有技术含量也非常经典，大家可以好好理解一下。

![img](https://p.ananas.chaoxing.com/star3/origin/d4a03394dd4e454c9462fa9f495d9c7d.png)

### 2.1.5 时序图

为了展示对象之间的交互细节，后续对设计模式解析的章节，都会用到时序图；

时序图（Sequence Diagram）是显示对象之间交互的图，这些对象是按时间顺序排列的。时序图中显示的是参与交互的对象及其对象之间消息交互的顺序。

时序图包括的建模元素主要有：对象（Actor）、生命线（Lifeline）、控制焦点（Focus of control）、消息（Message）等等。

 **角色（Actor）：**   系统角色，可以是人、及其甚至其他的系统或者子系统。

 **对象（Object）：**  对象包括三种命名方式：

-   第一种方式包括对象名和类名；
-   第二中方式只显示类名不显示对象名，即表示他是一个匿名对象；
-   第三种方式只显示对象名不显示类明。

![SequenceDiagram.jpg](https://p.ananas.chaoxing.com/star3/origin/3d4ad77cf16fc64667e0119e3afe2afb.jpg)

 **生命线（Lifeline）：**  生命线在顺序图中表示为从对象图标向下延伸的一条虚线，表示对象存在的时间，如下图

![lifeline.gif](https://p.ananas.chaoxing.com/star3/origin/751e54d02d74a97919dd96625caa3dbe.gif)

 **控制焦点（Focus of Control）**：  控制焦点是顺序图中表示时间段的符号，在这个时间段内对象将执行相应的操作。用小矩形表示，如下图。

![ElementFOC.jpg](https://p.ananas.chaoxing.com/star3/origin/ebfdc7233927ef3a60cbf9da3de617f7.jpg)

**消息（Message）**：  消息一般分为同步消息（Synchronous Message），异步消息（Asynchronous Message）和返回消息（Return Message）.如下图所示：

![Message.gif](https://p.ananas.chaoxing.com/star3/origin/9a63cede09efb161e9a93b753f296554.gif)

同步消息=调用消息（Synchronous Message）：  消息的发送者把控制传递给消息的接收者，然后停止活动，等待消息的接收者放弃或者返回控制。用来表示同步的意义。

 异步消息（Asynchronous Message）：  消息发送者通过消息把信号传递给消息的接收者，然后继续自己的活动，不等待接受者返回消息或者控制。异步消息的接收者和发送者是并发工作的。

  返回消息（Return Message）：  返回消息表示从过程调用返回

  自关联消息（Self-Message）：  表示方法的自身调用以及一个对象内的一个方法调用另外一个方法。

![SelfMessage.gif](https://p.ananas.chaoxing.com/star3/origin/31dca6f1c4828e193dd3834b7fc1323e.gif)

组合片段示例：

![CombinedFragments.gif](https://p.ananas.chaoxing.com/star3/origin/aa735c6b2c20dc9a0c35a67e9e42dcca.gif) 

- Alternative fragment（denoted “alt”） 与 if…then…else对应
- Option fragment (denoted “opt”) 与 Switch对应
- Parallel fragment (denoted “par”) 表示同时发生
- Loop fragment(denoted “loop”) 与 for 或者 Foreach对应

**时序图实例分析（Sequece Diagram Example Analysis）**

完成课程创建功能，主要流程有：

1、请求添加课程页面，填写课程表单，点击【create】按钮

2、添加课程信息到数据库

3、向课程对象追加主题信息

4、为课程指派教师

5、完成课程创建功能

![Dequence_Diagram_Example.jpg](https://p.ananas.chaoxing.com/star3/origin/6381e4331f4ce0b365690111bdd6b04a.jpg)

1、序号1.0-1.3  完成页面的初始化

2、序号1.4-1.5  课程管理员填充课程表单

3、序号1.6-1.7  课程管理员点击【Create】按钮，并响应点击事件

4、序号1.8     Service层创建课程

5、序号1.9-1.10 添加课程到数据库，并返回课程编号CourseId

6、序号1.11-1.12 添加课程主题到数据库，并返回主题编号topicId

7、序号1.13         给课程指派教师

8、序号1.14         向界面抛创建课程成功与否的消息