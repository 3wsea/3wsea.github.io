---
title: Jmeter之JSON提取器应用
date: 2021-09-09 11:32:00 +0800
categories: [JMeter, 基础]
tags: [JMeter基础]
pin: false
---

在接口测试中有一个这样的场景：登录之后，需要进行昵称修改，怎么实现？

首先我们分别看下登录、昵称修改的接口说明：

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-1.png)

![图2](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-2.png)

以上业务中补充一点，昵称修改，还需要添加请求头Authorization传登录获取的token值。

**分析：**登录之后的响应结果中会返回用户id、token信息; 而更新昵称需要传参member\_id、且需要请求头传token；也就是我们要想办法从“登录”的响应结果中获取到id、token信息，再去传给“更新昵称”请求。因为返回数据格式是json，所以我们用JSON提取器来实现。用正则表达式提取器也可以，大家可参见我之前写的文章。

### **一、json提取器设置多个变量获取多个数据**

**1、先添加好登录请求，运行下查看登录的响应结果**

![图3](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-3.png)

调整下查看结果方式，可较清晰看到响应结果中的id、token信息

![图4](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-4.png)

JSON path表达式怎么写？例如我要获取id，可写$.data.id，$表示根元素，然后一级级属性往下去找，先找到data，再往下子节点找到id；也可写$..id，直接从根元素去递归查找到id；获取token同理。

![图5](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-5.png)

![图6](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-6.png)

**2、右键登录请求-->后置处理器-->添加“JSON提取器”。**![图7](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-7.png)

**JSON提取器说明：**

**Apply to：**应用范围

**Names of created variables** ：接收值的变量名，自定义，多个变量用分号分隔 

**JSON Path expression：** json path表达式，也是用分号分隔 

**Match No.(0 for Random)：**0表示随机；n取第几个匹配值；-1匹配所有。若只要获取到匹配的第一个值，则填写1

**Compute concatenation var(suffix\_ALL):**如果找到许多结果，则插件将使用'，'分隔符将它们连接起来，并将其存储在名为\<variable name\>_ALL的var中

**Default Values：**缺省值，匹配不到值的时候取该值，可写error。

为了检查通过json提取器是否正常获取到值，可添加Debug PostProcessor来检查，注意正常跑用例时删除或禁用它。

![图8](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-8.png)

对比登录返回的结果，能看到已经正常获取到id，token。

**3、添加“修改昵称”请求，将json提取器获取到的值，进行变量引用。运行脚本，更改昵称成功。**

![图9](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-9.png)

![图10](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-10.png)

![图11](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-11.png)

### **二、json提取器设置1个变量获取多个数据**

我们也会碰到获取一个变量，但结果是多个数据的情况。

**1、例如“获取用户列表”，先来看下“获取用户列表”的响应结果。**

![图12](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-12.png)

如果想要获取到这里所有的用户id，json路径表达式怎么写呢？$..id或者$.data\[\*\].id

![图13](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-13.png)

**2、添加JSON提取器，获取所有用户id信息。**

![图14](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-14.png)

添加Debug PostProcessor来调试用，正常跑用例删除或禁用它。可查看到获取到了所有用户id值。通过id\_1,....id\_22可得到每一个id。

![图15](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-15.png)

**3、将获取到每一个用户id循环进行“用户流水记录”查看结果。**

1）添加循环控制器，设置循环次数。

![图16](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-16.png)

2）在循环控制器之下添加“用户流水记录”请求，并做以下设置。

![图17](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-17.png)

因为要把JSON提取器中匹配到的id_1,....id_22循环传递给“用户流水记录”，

__counter为jmeter自带的一个计数函数，${__counter(,)}引用这个函数，每运行1次+1，从1开始；

id_${__counter(,)}运行第1次id_1，运行第2次id_2，以此类推，直到运行到循环次数id_22

__V为jmeter自带的一个嵌套变量函数，运行第1次id_${__counter(,)}为id_1，${__V(id_${__counter(,)},)}则为${id_1}，值80；以此类推，一直循环到${id_22}

![图18](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-18.png)

以上为jmeter工具栏tools-->函数助手对话框界面，可以去选择帮助查看对应函数的功能说明。

3）运行后，可查看到结果

![图19](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8-19.png)