---
title: Jmeter 之测试片段、Include Controller、模块控制器应用
date: 2021-09-09 10:27:00 +0800
categories: [JMeter, 基础]
tags: [JMeter基础]
pin: false
---

### **一、测试片段是什么？**

应用在控制器上的一个特殊线程组，与线程组处于同一层级，必须与Include Controller或模块控制器一起使用才被执行。

### **二、应用场景**

1、当jmeter脚本非常复杂的时候，可以通过测试片段分模块管理用例

2、当jmeter脚本由多个测试人员共同完成，通过测试片段分人分模块管理用例

### **三、新建测试片段**

##### 1、右键测试计划->添加Test Fragment

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-1.png)

![图2](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-2.png)

##### 2、在测试片段下，整理好测试用例脚本；

![图3](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-3.png)

##### 3、保存测试片段脚本。选择文件—>保存测试计划为—>保存到本地。

![图4](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-4.png)

##### 4、其他模块整理的用例同理按上面1、2、3步来新建保存。

### **四、使用Include Controller调用测试片段**

Include Controller可以直接引用测试片段保存的jmx文件，但如果使用Cookie管理器或用户定义的变量，则需要将这些变量放在顶级测试计划中，而不是包含的jmx文件中，否则无法保证运行正常。

重新打开一个jmeter窗口，添加好线程组、用户定义变量、Include Controller、查看结果树。

##### 1、用户定义变量设置好访问的ip及port

![图5](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-5.png)

##### 2、Include Controller中选择本地保存好的测试片段脚本

![图6](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-6.png)

ps：这些测试片段的脚本可以集中管理在jmeter的bin目录下，在bin目录下新建一个项目文件夹，实现测试用例集中管理；Include Controller中修改为相对路径

![图7](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-7.png)

![图8](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-8.png)

##### 3、运行，查看结果树能看到测试结果

![图9](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-9.png)

### **五、使用模块控制器调用测试片段**

模块控制器提供了一种在运行时将测试片段替换为当前测试计划的机制。测试片段可以位于任何线程组中。

##### 1、打开一个jmeter窗口，添加好线程组、用户定义变量、模块控制器、测试片段、查看结果树。

##### 2、用户定义变量同样定义好访问ip及port端口号，以下用户模块、充值模块为测试片段

##### 3、同一个线程组中，有多个测试片段，只需通过模块控制器下拉框中选择适当的控制器，即可在这些多个测试用例之间轻松切换。但注意这些片段名称必须唯一。

1）模块控制器只选择用户模块，则只运行用户模块的测试片段

![图10](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-10.png)

![图11](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-11.png)

2）模块控制器只选择充值模块，则只运行充值模块的测试片段

![图12](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-12.png)

![图13](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-13.png)

##### 4、若片段位于线程组中且可运行，则需要禁用其Controller以防止片段运行，模块控制器除外。

![图14](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-14.png)

![图15](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-15.png)

运行结果：

![图16](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4-16.png)