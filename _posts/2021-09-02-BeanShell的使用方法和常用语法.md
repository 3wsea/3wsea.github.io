---
title: JMeter中BeanShell的使用方法和常用语法
date: 2021-09-02 16:13:00 +0800
categories: [JMeter, BeanShell]
tags: [JMeter, BeanShell]
pin: false
---

### **一、什么是BeanShell**

**BeanShell**是由java编写的，是一个轻量级的脚本语言，也相当于一个小巧免费的JAVA源码解释器，支持对象式的脚本语言特性，亦可嵌入到JAVA源代码中，能动态执行JAVA源代码并为其扩展了脚本语言的一些特性。

### **二、JMeter中用BeanShell的好处**

JMeter也是由java编写的，而java运行时需要先编译，然后才可以运行，而BeanShell是一款解释器，直接可能运行源代码；

### **三、BeanShell在JMeter的作用**

（1）对JMeter的功能的扩展；

（2）对jmeter变量的一些操作；

### **四、JMeter中BeanShell常用的基本语法：**

    1、log：打印日志，写入信息到jmeter.log文件；  
  
    2、SampleResult：获取SampleResult对象，能通过这个对象获取想要的信息；  
  
    3、Response：获取Response对象，能通过这个对象获取响应信息；  
  
    4、Failure：查看接口调使用能否成功，假如返回false是成功的，true是失败的。  
  
    5、FailureMessage：失败信息，没有设置的时候失败信息是空的，能set这个信息。  
  
    6、ResponseData：获取response body类型是byte\[\]。  
  
    7、ResponseCode：返回接口code成功是200。  
  
    8、ResponseMessage：获取msg成功是OK。  
  
    9、ResponseHeaders：获取接口服务端返回的头部信息。  
  
    10、RequestHeaders：获取用户端请求的头部信息。  
  
    11、SampleLabel：获取接口请求的名称。  
  
    12、SamplerData：获取请求的url和body。  
  
    13、ctx 当前线程的上下文信息，能直接用  
  
    14、vars即JMeterVariables，操作jmeter变量，这个变量实际引用了JMeter线程中的局部变量容器(本质上是Map),常用方法：

        (1) vars.get("String key")：从jmeter中获得变量值；  
  
        (2) vars.put("String key"，"String value")：将数据存到jmeter变量中；  
  
    15、prev 获取sample返回的信息，常用方法：  
  
        (1)getResponseDataAsString()：获取响应信息。  
  
        (2) getResponseCode()：获取响应code。

### **五、Jmeter中包含的BeanShell类型** 
 
    1、BeanShell前置处理器：BeanSell PreProcessor  
    2、BeanShell取样器：BeanSell Sampler  
    3、BeanShell后置处理器：BeanSell PostProcessor  
    4、BeanShell监听器：BeanSell Listener  
    5、BeanShell定时器：BeanSell Timer  
    6、BeanShell断言：BeanSell断言

**BeanShell前置处理器、BeanShell取样器、BeanShell后置处理器，它们之间的区别**：

1、BeanShell前置处理器、BeanShell后置处理器 比 BeanShell取样器多一个重置解释器（Reset Interpreter），意思是指：对于每一次处理是不是都要进行重置；

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-3.png)

2、在JMeter中BeanShell取样器是当成线程处理的，所以运行时是有一定的消耗的，而使用BeanShell前置处理器、BeanShell后置处理器总体来说比BeanShell取样器消耗低，相当于少起一个线程。

### **六、JMeter中BeanShell使用**

**1、BeanSell取样器中直接编写代码**

（1）提取下图中的https的链接地址

![图2](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-4.png)

（2）首先用正则表达式提取器来提取值，这边的正则表达式可以只提取一个值，也就是只写了一个()

![图3](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-5.png)

（3）在`BeanShell`取样器输入输入源码，注意：这边一定要写参数`${ABCDEF}`，这个值依赖于正则表达式提取数据，并且在代码中一定要写`str = bsh.args\[0\]`，表示获取数组参数，如果是取变量，用`vars.get("String key")`

![图4](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-6.png)

```java
public static void link() {
        String IP;
        String path;
        String str;
        str = bsh.args[0];
        IP = str.substring(0, str.indexOf("/"));
        path = str.substring(str.indexOf("/") + 1);
        vars.put("IP", IP);
        vars.put("path", path);
    }
    link();
```

（4）`Http2`请求中服务器与路径的数据依赖于`BeanShell`取样器中所获取的`IP`、`path`的值

![图5](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-7.png)

（5）运行后，在察看结果树中，可以看到获取到了`IP`与`path`的值

![图6](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-8.png)

（6）察看结果树中显示`Http2`请求数据正确

![图7](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-9.png)

**2、引用外部的java源码**

（1）java源码，右键--Properties，查看java源码的存放路径

![图8](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-10.png)

```java
public class BeanShell {
        public static String[] beanShellMethod(String str) {
            String[] arr = new String[2];
            //截取/前的字符串
            arr[0] = str.substring(0, str.indexOf("/"));
            //截取/后的字符串
            arr[1] = str.substring(str.indexOf("/") + 1);
            return arr;
        }
    }
```

（2）BeanShell取样器中要引入java源码的存放路径，`source("E:/jmeterMethod/BeanShell.java")`;

![图9](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-11.png)

```java
source("E:/jmeterMethod/BeanShell.java");
String str = bsh.args[0];
BeanShell method = new BeanShell();
String[] array = method.beanShellMethod(str); 
for(int i = 0;i<array.length-1;i++){
    String IP = array[i];
    String path = array[i + 1];
    vars.put("IP", IP);
    vars.put("path", path);
}
```

(3)Http2请求中服务器与路径的数据依赖于BeanShell取样器中所获取的IP、path的值

![图10](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-12.png)

(4)运行后，在察看结果树中，可以看到获取到了IP与path的值

![图11](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-13.png)

**3、引入外部class文件**

(1)DOS中将源码编译成class文件

![图12](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-14.png)

（2）BeanShell取样器，引入class文件用addClassPath("相对路径或绝对路径")，还需用import导入，格式为： `import 包名.类名`，注意：包名一定要写上。

注意：addClassPath()是找指定的路径，source()是找指定的文件

![图13](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-15.png)

```java
addClassPath("F:\\java\\j\\p\\src"); 
import com.jyh.BeanShell; 
String str = bsh.args[0]; 
BeanShell method = new BeanShell(); 
String[] array = method.beanShellMethod(str); 
for(int i=0;i<array.length-1;i++) {
    String IP = array[i];
    String path = array[i+1];
    vars.put("IP",IP);        
    vars.put("path",path);
}
```

(3)运行后，在察看结果树中，可以看到获取到了IP与path的值

![图14](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-16.png)

4、引入外部jar包

(1)将jar包复制到jmeter\\lib\\ext路径下，这个lib\\ext路径下存放引入外部的一些jar包

(2)BeanShell取样器，注意一定要用import导入包

![图15](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-17.png)

```java
import com.jyh.BeanShell; 
String str = bsh.args[0]; 
BeanShell method = new BeanShell(); 
String[] array = method.beanShellMethod(str); 
for(int i=0;i<array.length-1;i++) {        
    String IP = array[i];
    String path = array[i+1];
    vars.put("IP",IP);        
    vars.put("path",path);
}
```

(3)运行后，在察看结果树中，可以看到获取到了IP与path的值

![图16](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210902/0902-18.png)