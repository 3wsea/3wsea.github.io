---
title: Jmeter之https脚本录制
date: 2021-09-09 09:51:00 +0800
categories: [JMeter, 基础]
tags: [JMeter基础]
pin: false
---

jmeter录制脚本时，跟http脚本录制主要区别是，https录制需要添加安全证书。

### **一、jmeter代理服务器及证书配置。**

1、打开jmeter，右键测试计划添加线程组，右键工作台-->非测试元件-->http代理服务器

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-1.png)

2、设置http代理服务器。

端口默认8888，为了避免跟本地fiddler冲突，改成了8088；

目标控制器选择，之前添加好的线程组；

排除模式添加 .\*\\.(js\|css\|PNG\|jpg\|png\|gif).\* ，过滤掉js、css、png、jpg等信息

![图2](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-2.png)

![图3](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-3.png)

3、启动代理服务器，会在jmeter的bin目录下生成一个安全证书文件：ApacheJMeterTemporaryRootCA.crt

![图4](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-4.png)

4、导入jmeter安全证书，在菜单选项--> ssl管理器，导入jmeter/bin目录下的安全证书文件ApacheJMeterTemporaryRootCA.crt，这样录制https的代理服务器就启动并设置成功了

![图5](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-5.png)

![图6](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-6.png)

### **二、浏览器的代理服务器及证书配置。**

1、 以火狐浏览器为例， 找到 证书管理器 -->导入jmeter中bin目录下的安全证书-->确定

![图7](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-7.png)

2、浏览器设置好代理配置，127.0.01，端口8088；且勾选为所有协议使用相同代理服务器，点击确定

![图8](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-8.png)

3、在浏览器中输入要访问的网址进行相关操作，就能在jmeter中看到录制的https脚本了

![图9](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-9.png)

  


注意：jmeter录制完脚本后，记得停止jmeter代理服务器，同时记得关闭浏览器代理配置，不然浏览器不能正常访问网站哟

![图10](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-10.png)

![图11](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210909/0909-11.png)