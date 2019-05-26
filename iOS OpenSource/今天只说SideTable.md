![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/iOS%20OpenSource/SideTable-Title.jpg)


## 写在前面

在开始我们今天的分享之前，先讲个这两天比较火的段子：
>昨晚做梦梦到我死了；  
>进了阎王殿；  
>阎王爷让我给他的生死薄做个后台管理系统。。。  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/iOS%20OpenSource/duanzi.png)


段子可能引起些许不适😅，但与我们今天分享的内容有关。  

阎王爷用生死薄记录人的生死轮回，而这个“生死薄”与SideTable有异曲同工之妙，SideTable同样肩负着管理、记录对象（Object）生死的重任。  

进入正题，本篇文章将会分为3部分。第一部分我会简要的分享一下当前高级语言内存管理的几种方式。第二部分会着重介绍下iOS内存管理方式。第三部分会重点分享SideTable数据结构。

## 高级语言常见内存管理方式

当前高级语言中，内存管理大致分为两大类：  

* 基于引用计数的内存管理方式。
* 基于垃圾回收机制的内存管理方式。
  
##### 基于引用计数的内存管理方式  












