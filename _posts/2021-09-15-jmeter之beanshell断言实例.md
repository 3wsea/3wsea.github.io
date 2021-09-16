---
title: JMeter之beanshell断言实例
date: 2021-09-15 11:36:00 +0800
categories: [JMeter, 基础]
tags: [JMeter基础]
pin: false
---


1.首先储存一个接口的响应结果，比如在http请求的后面添加beanshell后置处理器(BeanShell PostProcessor)来储存http请求的响应结果：


```java
import org.json.*;

//获取上一个请求的返回值
String response = prev.getResponseDataAsString();
//将返回值转换为json
JSONObject responseJson = new JSONObject(response);
//获取responseMessage
String message = responseJson.getString("responseMessage");
log.info("message的值：" + message);

//使用vars.put()方法储存变量message
vars.put("message",message);
//获取titleLink
String titleLink = responseJson.getJSONObject("data").getString("titleLink");
log.info("titleLink的值:" + titleLink);
//使用vars.put()方法储存变量message
vars.put("titleLink",titleLink);
```


在后面的其他接口中如何需要使用变量message和titleLink，可以使用${message}和${titleLink}来获取变量的值；

变量储存好后，在需要断言的接口后面添加BeanShell断言，使用Failrue来标识断言失败，FailureMessage标示断言失败的原因，如：


```java
//使用vars.get()方法获取变量的值
String message= vars.get("message");

if(!message.equals("success")) {
    Failure = true;
    FailureMessage = "规则解析失败";
}else{
    FailureMessage = "规则解析成功";
    }
```

2.直接在需要断言的接口后面使用beanshell断言，使用Failrue来标识断言失败，FailureMessage标示断言失败的原因，如：


```java
import org.json.*;

//获取上一个请求的返回值
String response = prev.getResponseDataAsString();
//将返回值转换为json
JSONObject responseJson = new JSONObject(response);
//获取responseMessage
String message = responseJson.getString("responseMessage");
log.info("message的值：" + message);

if(!message.equals("success")){
    Failure = true;
    FailureMessage = "规则解析失败，message不等于success";
    return;
}

//获取titleLink
Object titleLink = responseJson.getJSONObject("data").get("titleLink");
log.info("titleLink的值:" + titleLink.toString());


if(titleLink.toString().equals("null") || "".equals(titleLink)){
    Failure = true;
    FailureMessage = "规则解析失败，titleLink为空";
}else if(!titleLink.toString().startsWith("http") && !titleLink.toString().startsWith("https")){
    Failure = true;
    FailureMessage = "规则解析失败，titleLink不为空，但是不是以http或者https开头的";
}
```

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210915/0915-1-1.png)

接口的响应数据为：

```json
{
   "responseCode":"1",
   "responseMessage":"success",
   "responseType":null,
   "data":
   {
      "city":"上海",
      "rentUnit":"月",
      "source":"个人房源网",
      "title":"徐盈路1188弄徐泾青浦徐泾租房",
      "belonger":"个人",
      "housingType":"住宅",
      "floor":"高层",
      "rentPrice":"17000",
      "titleLink":"http://sh.grfy.net/rent/d-34612565.html",
      "decoration":null,
      "direction":null,
      "isSplit":"否",
      "imgs":null,
      "publishTime":"2018-07-25T23:20:33.471",
      "contactMobile":null,
      "website":"http://sh.grfy.net/rent/list_2_0_0_0-0_0_0-0_0_2_0_{}_.html",
      "address":"徐泾",
      "contactName":"王女士",
      "houseType":"4室2厅2卫",
      "estate":"徐泾",
      "roomArea":"177",
      "collectHouseType":"住宅",
      "collectType":"出租",
      "district":"青浦",
      "totalFloor":"共20层",
      "region":"上海",
      "isRegister":"否",
      "desc":"仁恒西郊花园 4室2厅2卫 房屋亮点 新上 配套齐全 有阳台 首次出租 随时看房 出租要求 一家人 一年起租 租户稳定 作息正常 房源描述小区环境好，物业管理成熟，私人会所实施配套齐全，临近地铁17号徐盈站，周边多所国际学校，仁恒的房子品质有保障。无中介费。"
   }
}
```



   断言失败如下：

![图1](https://cdn.jsdelivr.net/gh/3wsea/blog-images@master/commons/20210915/0915-1-2.png)