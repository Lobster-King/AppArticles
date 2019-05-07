## 写在前面  
关于软件架构模式（确切的说是一种软件编码规范或者软件开发模式），这几年骂战不断。争论的焦点主要是在MVC、MVVM、MVP哪种架构最好，哪种架构才是最牛逼的、扩展性更强的、可维护性更高的。笔者不才，在实际项目中很少用过MVP架构，对于MVP的掌握也是只停留在写写Demo阶段。本篇文章主要着重介绍下MVVM架构在真实项目当中的应用，以及抛开RAC，我们如何自己动手写一个view和viewModel之间的绑定框架。  

## MVVM扫盲  
>MVVM（Model–view–viewmodel）是一种软件架构模式。

>MVVM有助于将图形用户界面的开发与业务逻辑或后端逻辑（数据模型）的开发分离开来，这是通过置标语言或GUI代码实现的。MVVM的视图模型是一个值转换器， 这意味着视图模型负责从模型中暴露（转换）数据对象，以便轻松管理和呈现对象。在这方面，视图模型比视图做得更多，并且处理大部分视图的显示逻辑。 视图模型可以实现中介者模式，组织对视图所支持的用例集的后端逻辑的访问。

>MVVM是马丁·福勒的PM（Presentation Model）设计模式的变体。 MVVM以相同的方式抽象出视图的状态和行为,但PM以不依赖于特定用户界面平台的方式抽象出视图（创建了视图模型）。
MVVM和PM都来自MVC模式。

>MVVM由微软架构师Ken Cooper和Ted Peters开发，通过利用WPF（微软.NET图形系统）和Silverlight（WPF的互联网应用派生品）的特性来简化用户界面的事件驱动程序设计。 微软的WPF和Silverlight架构师之一John Gossman于2005年在他的博客上发表了MVVM。

>MVVM也被称为model-view-binder，特别是在不涉及.NET平台的实现中。ZK（Java写的一个Web应用框架）和KnockoutJS（一个JavaScript库）使用model-view-binder。  

以上内容来自维基百科。[MVVM](https://zh.wikipedia.org/wiki/MVVM)。  
简单的讲，