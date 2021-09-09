---
title: Jmeter 中 CSV 如何参数化测试数据并实现自动断言
date: 2021-09-09 13:21:00 +0800
categories: [JMeter, 基础]
tags: [JMeter基础]
pin: false
---

当我们使用Jmeter工具进行接口测试，可利用CSV Data Set Config配置元件，对测试数据进行参数化，循环读取csv文档中每一行测试用例数据，来实现接口自动化。此种情况下，很多测试工程师只会人工地查看响应结果来判断用例是否通过。  
   其实我们同样可利用CSV Data Set Config来帮助我们实现自动断言。  
   **思路：**将每一条用例的预期结果一并保存在csv文档中，循环读取文档中的期望结果，来跟实际运行的结果进行一致性判断，高效实现接口自动化。

**示例：**

### **1、整理测试数据及预期结果的CSV文档** 

比如当前要去测试一个注册接口，先来分析下注册传入哪些参数，有包括手机号码mobile\_phone、密码Pwd、用户类型type、注册名reg\_name；分析注册返回结果并取响应结果中参数code、msg实现断言。  
将注册中每条用例需要传入的这4个参数数据，以及需要断言的期望的code、msg整理到一个csv文档中，如下：  

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-1.png)

### **2、CSV测试数据参数化**

打开jmeter，添加好线程组、信息头管理器、注册请求、CSV数据文件设置、查看结果树；

#### 1)CSV数据文件设置

![图2](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-2.png)

以上注意：若CSV中数据包含中文，这里设置文档默认编码GBK。

#### 2)线程组设置

一般CSV文档中有几条用例，就设置线程数为几。这里有5条用例数据，所以就设置为5

![图3](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-3.png)

#### 3)注册请求变量引用

注册请求传入的json数据中，将值的部分进行${变量名}。

![图4](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-4.png)

### **3、设置断言**

在注册请求下添加响应断言，因为接口返回的响应结果为json格式；所以这里响应内容设置为"code":${code},"msg":"${msg}"  
其中**${code},${msg}**为读取到的CSV文档中的值

![图5](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-5.png)

### **4、运行，查看结果树及断言结果**

可看到针对每一条用例都分别进行了预期结果跟实际结果的一致性判断，判断通过则显示绿色，判断不通过，显示红色。

![图6](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9-6.gif)