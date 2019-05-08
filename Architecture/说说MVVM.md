## 写在前面  
关于软件架构模式（确切的说是一种软件编码规范或者软件开发模式），这几年骂战不断。争论的焦点主要是在MVC、MVVM、MVP哪种架构最好，哪种架构才是最牛逼的、扩展性更强的、可维护性更高的。笔者不才，在实际项目中很少用过MVP架构，对于MVP的掌握也是只停留在写写Demo阶段。本篇文章主要着重介绍下MVVM架构在真实项目当中的应用，以及抛开RAC，我们如何自己动手写一个view和viewModel之间的绑定框架。  

## MVVM扫盲  
>MVVM（Model–view–viewmodel）是一种软件架构模式。

>MVVM有助于将图形用户界面的开发与业务逻辑或后端逻辑（数据模型）的开发分离开来，这是通过置标语言或GUI代码实现的。MVVM的视图模型是一个值转换器， 这意味着视图模型负责从模型中暴露（转换）数据对象，以便轻松管理和呈现对象。在这方面，视图模型比视图做得更多，并且处理大部分视图的显示逻辑。 视图模型可以实现中介者模式，组织对视图所支持的用例集的后端逻辑的访问。

>MVVM是马丁·福勒的PM（Presentation Model）设计模式的变体。 MVVM以相同的方式抽象出视图的状态和行为,但PM以不依赖于特定用户界面平台的方式抽象出视图（创建了视图模型）。
MVVM和PM都来自MVC模式。

>MVVM由微软架构师Ken Cooper和Ted Peters开发，通过利用WPF（微软.NET图形系统）和Silverlight（WPF的互联网应用派生品）的特性来简化用户界面的事件驱动程序设计。 微软的WPF和Silverlight架构师之一John Gossman于2005年在他的博客上发表了MVVM。

>MVVM也被称为model-view-binder，特别是在不涉及.NET平台的实现中。ZK（Java写的一个Web应用框架）和KnockoutJS（一个JavaScript库）使用model-view-binder。  

以上内容来自维基百科。[MVVM wikipedia](https://zh.wikipedia.org/wiki/MVVM)。  

简单的讲，MVVM是MVC的改进版。我们都知道MVC软件架构模式是苹果推荐的开发模式。**MVC**中的**M**就是单纯的从网络获取回来的数据模型，**V**指的我们的视图界面，而**C**就是我们的ViewController。在其中，ViewController负责View和Model之间调度，View发生交互事件会通过target-action或者delegate方式回调给Controller，与此同时Controller还要承担把Model通过KVO、Notification方式传来的数据传输给View用于展示的责任。***随着业务越来越复杂，视图交互越复杂，导致Controller越来越臃肿，负重前行。脏活累活都它干了，到头来还一点不讨好。福报修多了结果就是，不行了就重构你，重构不了就换掉你。***😅

来一张斯坦福老头经典的MVC架构图。  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/mvc-arch.jpg)  

所以为了解决这个问题，MVVM就闪亮登场了。他把View和Contrller都放在了View层（相当于把Controller一部分逻辑抽离了出来），Model层依然是服务端返回的数据模型。***而ViewModel充当了一个UI适配器的角色，也就是说View中每个UI元素都应该在ViewModel找到与之对应的属性。除此之外，从Contrller抽离出来的与UI有关的逻辑都放在了ViewModel中，这样就减轻了Contrller的负担。***  

我简单的画了下MVVM的架构图。  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/mvvm-arch.png)   

从以上的架构图中，我们可以很清晰的梳理出各自的分工。  

* **View层**：视图展示。包含UIView以及UIViewController，View层是可以持有ViewModel的。
* **ViewModel层**：视图适配器。暴露属性与View元素显示内容或者元素状态一一对应。一般情况下ViewModel暴露的属性建议是readOnly的，至于为什么，我们在实战中会去解释。还有一点，ViewModel层是可以持有Model的。
* **Model层**：数据模型与持久化抽象模型。数据模型很好理解，就是从服务器拉回来的JSON数据。而持久化抽象模型暂时放在Model层，是因为MVVM诞生之初就没有对这块进行很细致的描述。按照经验，我们通常把数据库、文件操作封装成Model，并对外提供操作接口。（有些公司把数据存取操作单拎出来一层，称之为**DataAdapter层**，所以在业内会有很多MVVM的变种，但其本质上都是MVVM。比较典型的就是猿题库的架构实践方案--->[猿题库iOS客户端架构设计](http://www.cocoachina.com/ios/20160108/14911.html)，它把数据存取和网络请求单拉出来形成DataContrller层，但从严格意义上来讲，猿题库的架构方案不算是MVVM的架构模式）。
* **Binder**：MVVM的灵魂。可惜在MVVM这几个英文单词中并没有它的一席之地，它的最主要作用是在View和ViewModel之间做了双向数据绑定。如果MVVM没有Binder，那它与MVC无异。

我们发现，正是因为View、ViewModel以及Model间的清晰的持有关系，所以在三个模块间的数据流转有了很好的控制。


## MVVM结合RAC

## MVVM结合非RAC

## 猿题库架构简介

## 对于开发者的建议

对于团队成员不是很多，项目又比较新的团队来说，我建议大家勇于尝试MVVM架构。  
而对于团队成员众多，项目遗留问题多的团队来说，我建议大家尝试MVVM+KVO+数据存取放到Model层的架构方案。其次，建议大家先尝试下猿题库的架构模式。猿题库的架构模式上手比较简单，但是Controller中也承担了View和ViewModel之间交互的职责。

## 实现一个View层和ViewModel层绑定框架

