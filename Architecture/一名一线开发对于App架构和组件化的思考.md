##写在前面
关于App架构、组件化，本文的内容不会涉及到具体代码层面，也不会介绍怎样使用Cocoapods去做组件化；而是站在软件工程的角度上，结合自己多年一线开发经验，去分析如何做App架构，如何通盘考虑什么样的架构才是合理的，契合自身业务的，以及架构落地过程中应该规避哪些问题。

>名词解释：本文中所提到的架构不是实际工程中代码架构（MVC、MVVM、MVP），确切的说是一种应用分层架构。而MVC、MVVM、MVP本质是一种软件架构模式，是App实现过程中的一种编码模式或者编码规范。

##iOS系统架构回顾

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/ios-architecture.png)

如上图所示，经典的iOS系统架构分为四层，自下而上分为核心操作系统层、核心服务层、媒体层、用户交互层。

* Cocoa Touch层：提供与用户交互的相关能力，包括触摸等，最常用的UIKit库就在该层；除此之外还有MapKit、Address BookUI、PhotosUI等。
* Media Layer层：提供图形与音视频相关功能的接口，例如Core Animation、OpenGL ES等。
* Core Services：最常用的有Core Foundation、Foundation、CFNetwork、CoreData等。提供最基础的能力，比如数组、字典、HashMap、套接字等数据结构。
* Core OS层：包含Mach、Kernel、BSD、Socket以及Sandbox等，提供底层操作的接口，对于应用开发来讲直接用到的不是很多。

>Q：iOS系统架构这四层之间是如何进行通信和交互的，是否合理？  
>A：直接引用头文件，调用下层提供的Api进行交互。只要Api设计的足够合理，足够能应对未来一段时间内SDK内部可能的变动，或者说SDK本身是一个很基础的库，比如Foundation库等，我觉得直接引入头文件无伤大雅，具体的我们稍后再讨论。











