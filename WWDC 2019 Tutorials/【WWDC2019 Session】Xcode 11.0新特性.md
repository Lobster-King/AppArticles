### 写在前面
**Apple WWDC 2019** 正如火如荼的进行着。按照WWDC计划，美国时间从Tuesday到Friday都会有各种Session分享（实在干货）。所有Session相关的Video、PPT都将陆续在 **[WWDC2019官网](https://developer.apple.com/wwdc19/schedule/#!/)** 放出。

撸主会根据Session Topic与实际工作的相关性并结合实际操作，陆续将Session内容第一时间分享给大家。

进入正题，本文分享的是Xcode 11新特性。  

>**PS：1.本文含有大量图片，主要是笔者实操的步骤截图，流量不够用的筒子们慎入！！2.Xcode 11 Beta版非常不稳定，经常闪退（WTF😠!!!），各位在实操的时候注意稳定情绪。3.因Beta版本的不稳定以及操作姿势可能不同，如果大家实操的结果与文中有出入，请及时留言交流反馈。**

### Xcode 11新特性

Xcode 11主要包含如下新特性：

* **Xcode工作流更新。**
* **Swift包管理工具（Xcode终于有了自己的可视化包管理工具了😅）。**
* **Source Control代码管理模块更新。**
* **设计工具。**
* **Debug工具更新。**
* **Testing模块更新。**
* **Simulator更新。**
* **Instruments性能分析工具。**
* **传新的UI框架SwiftUI。**

下面分别介绍这些新特性。

#### 一、Xcode工作流更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/workflow.png)

**Xcode 10右上导航的样子** 

![](https://github.com/Lobster-King/AppArticles/raw/master/WWDC%202019%20Tutorials/xcode10navi.png)

**Xcode 11右上导航的变化**  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xcode11navi.png)

**Library变化**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xocde11library.png)

**新增Editor Options和Add Editor、以及MiniMap**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/minimap.png)

**其他重要能力提升**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/codecompletion.png)

**主要的变化总结如下：**

* Library（代码块Snippets和图片资源预览），由花括号改成了“+”号。原先Snippets、图片预览两项功能扩展为4项：1.SwiftUI控件库。2.SwiftUI Modifiers。3. Snippets代码块。4.图片资源预览（包括后面提到的SPM中的图片资源、Symbols）。5.Color选择器。

* Version Editor中把Authors（以作者为视角查看代码更改记录）、Logs（以提交时间为视角查看代码更改记录）移出（集成到Editor Options中），只保留Code Review（Comparison代码比对）功能。

* 新增Editor Options和Add Editor功能区。其中Editor Options包含：展示/隐藏SwiftUI画布、展示/隐藏Authors视角、MiniMap等。Add Editor可以新增多个编辑窗口，终于不需要几个编辑窗口来回切换了。

* 新增MiniMap功能。有了代码地图预览功能，查找代码不再繁琐。搜索代码，MiniMap也同步高亮显示。鼠标在MiniMap移动时，也会有方法高亮提示。

* 其他重要功能能力提升。主要包括：代码补全能力提升、拖拽代码能力提升、拼写检查等。（由于时间和篇幅原因这部分功能笔者暂未实践）

**总结：从上面的更新Tips来看，Xcode正在往好的方向发展，之前饱受开发者诟病的沙雕功能也已慢慢被Apple优化。MiniMap、和Add Editor是个亮点，有了这两个重要功能加持，跟其他IDE相比起码在易用性上不落下风。**

#### 二、Swift包管理工具（SPM）

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/swiftpackagemanager.png)

Xcode终于有了自己的可视化包管理工具了！！！喜大普奔，笔者赶紧尝试了一下SPM可视化工具。

**SPM入口**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/spmentrance.png)  

用Xcode 11打开工程，点击Project，在Info、Build Setting后面多了一个Swift Packages选项，这就是SPM入口。

点击+号，Xcode会提示你绑定一个账号： **BitBucket、GitHub、GitLab**

笔者绑定的是GitHub账号，Xcode会按照时间顺序自动检索出该账号下的repositories。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/repos.png)

**添加一个repository到工程（笔者加入的是Alamofire Swift网络库）**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/thirdlibrary.png)

添加完成之后，会在文件目录的最下方出现**Swift Package Dependencies目录**

**使用新加入的repository**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/libraryusage.png)

在Appdelegate.m中引入刚添加的Swift Package->Alamofire。发起一个GET请求，打印出返回的HTML字符串，完美运行。

**适配现有工程、创建Swift Package库、二进制库等等**

这部分内容笔者会尽快去尝试，然后拿出来分享给大家。除了以上的内容，还有几个问题需要我们去思考并尝试的。

* SPM如何管理包之间的依赖问题？
* SPM如何进行包的版本控制？
* SPM如何适配私有Swift库？

**总结：跟SPM命令行工具相比可视化包管理工具更加简单直观。（是不是可以理直气壮的跟Cocoapods、Carthage说拜拜了？可能需要很长的路要走。。。😅）。SPM的出现具有划时代的意义，体现了Apple致力于完善Swift生态的坚定决心。**


#### 三、Source Control代码管理模块更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/sourcecontrol.png)

相信大多数的iOS开发者很少使用Xcode自带的Git GUI工具（实在太鸡肋，功能少的可怜），一般用Source Tree或者Command Line居多。而且在Xcode 10以前，Xcode Git GUI工具不支持stash、cherry-pick命令，在Xcode 11中加入了这两项功能。

在Source Control下拉菜单中加入了Cherry-Pick、Stash Changes选项。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/stashcherry-pick.png)

**Stash Changes**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/stashcomment.png)

选择Stash Changes选项，会自动弹出一个对话框，提示我们输入一些Comment信息。点击Stash按钮自动跳转到Git管理Tab下。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/stashinfo.png)

在Git Tab页面，会展示出当前本地Branches列表、Stash Changes列表、Tags列表、Remotes远程仓库列表。

在Stash右下角会有Stash功能选择按钮（也可以在左侧Stash Changes右键弹出）。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/stashoption.png)

简要说下三个选项：

* **Apply Stashed Changes**：跟git stash apply作用一样，会把当前Stash Changes栈顶元素应用到当前分支，Stash Changes栈不会移除这个Stash。
* **Export Stashed Changes as Patch File**：将某个Stash Changes导出一个Patch文件。
* **Delete**：跟git stash pop作用一样，将Stash Changes栈顶元素应用到当前分支，Stash Changes栈会移除这个Stash。


**Cherry-Pick**

假设有两个分支A和B，进行合并分支的时候，你并不想把B的所有commit合并到A，而是想把单个commit合并过来，这时候就得用到git cherry-pick命令了。

![](https://github.com/Lobster-King/AppArticles/raw/master/WWDC%202019%20Tutorials/cherry-pick.png)

选择某个commit，然后cherry-pick，弹出框点击cherry-pick就会将该commit合并到master分支，并生成一个新commit。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/cherry-pickoption.png)

**总结：Xcode Git GUI工具虽然加入了Stash、Cherry-pick功能，但对某些版本管理需求负责的业务场景远远不够， 而对于基本的开发来讲完全够用了。**

#### 四、设计工具

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/designtool.png)

**暗黑模式切换操作区**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/storyboardtoolbar.png)

**Symbols（另类Icon Font）**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/symbols.png)

在Library中，可以选择Symbols元素。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/symbolsconfig.png)

Xcode 11竟然也有自己的标准图片Icon库了，而且可以进行Symbols Configuration，**Icon Font**即视感有没有！！！

**自定义图片暗黑模式适配**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/customimage.png)

在Asset Catalog中添加自定义图片，然后选择Appearances，然后添加暗黑模式下的图片即可。

**总结：感觉Xcode 11中，不管SwiftUI还是Symbols，都跟Flutter正面刚上了啊😅。。不知接下来Xcode是不是也会提供类似于Material Design风格的控件集。官方Session中提到，Symbols会自动适配iOS 13暗黑模式（这个笔者实验了下，Icon并没有跟随模式自动切换，知道怎么设置的同志可以留言告诉我一下）。**

>**PS：在设计工具Topic分享中，出现了位女中豪杰->Lisa Xiao。不知是否是中国人，只是觉得真🐂🍺。**

#### 五、Debug工具更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/debugging.png)

**网络模拟测试**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/realdevice.png)

之前真机网络测试只能在手机上操作，现在可以直接在**Xcode->Devices & Simulators**中进行测试。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/realdevicecontrol.png)

可以点击真机左上角小按钮来停止测试。

**总结：Xcode 11进行网络测试更方便**

#### 六、Testing模块更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/testing.png)

* **Reusable across schemes：通过配置不同的schemes来实现复用。**
* **Define what tests to include：自定义测试所包含的内容。**
* **Specify multiple configurations：支持多种配置来进行白盒测试。**
* **支持iPad App for Mac以及SwiftUI的测试。**


Testing加入了Test Server的模块，用来把测试用例一次性运行在不同的平台上（iPhone、iPad、Mac App）。

**总结：一次编写+一次配置=多端可测。Testing的相关更新，笔者尚未具体实践，有兴趣的筒子可以去体验一把，貌似国内基于TDD开发模式的团队少之又少吧。。。**

#### 七、Simulator更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/simulator.png)

**Watch App模拟器更独立，可以单独运行**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/watch.png)

**利用Metal重构Simulator，且Metal程序可以直接在模拟器中运行**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/simulatorperformance.png)

官方给出的Simulator性能数据：

* 模拟器FPS达到60帧每秒。
* CPU占用率减少至少90%。
* 热启动至少提速2倍以上。

**总结：Watch Simulator独立出来，可以预见Apple对于可穿戴设备的重视程度，除此之外Apple Wacth还会有自己独立的Watch App Store。另外Simulator利用Metal重构，其在性能上的表现确实让人眼前一亮。是不是要开始Metal框架的学习了呢？😅**

#### 八、Instruments性能分析工具

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/instruments.png)

**增强分类信息展示**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/category.png)

**新增SwiftUI Instrument模版**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/swiftuitemplate.png)

**增强Metal System Trace模版数据分析性能（数据分析速度提高10倍以上）**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/metaltemplate.png)

#### 九、传新的UI框架SwiftUI

两个字：🐂🍺

后续会继续出一些关于SwiftUI的相关分享。

**文章首发GitHub [https://github.com/Lobster-King/AppArticles](https://github.com/Lobster-King/AppArticles)**
