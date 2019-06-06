### 写在前面
**Apple WWDC 2019** 正如火如荼的进行着。按照WWDC计划，美国时间从Tuesday到Friday都会有各种Session分享（实在干货）。所有Session相关的Video、PPT都将陆续在 **[WWDC2019官网](https://developer.apple.com/wwdc19/schedule/#!/)** 放出。

撸主会根据Session Topic与实际工作的相关性并结合实际操作，陆续将Session内容第一时间分享给大家。**因Beta版本的不稳定以及操作姿势的不同，可能会导致大家实操的结果与文中有出入，所以有问题请大家及时反馈。**

进入正题，第一篇分享的是Xcode 11新特性。  

>**PS：Beta版Xcode 11非常不稳定，经常闪退，各位在实操的时候注意稳定情绪。WTF😠!!!**

### Xcode 11新特性

Xcode 11主要包含如下新特性：

* **Xcode工作流更新。**
* **Swift包管理工具（Xcode终于有了自己的包管理工具了😅）。**
* **Source Control代码管理模块更新。**
* **StoryBoard设计工具。**
* **Debug工具更新。**
* **Testing模块更新。**
* **Instruments性能分析工具。**
* **全新的UI框架SwiftUI。**

下面分别介绍这些新特性。

#### 一、Xcode工作流更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/workflow.png)

**先来看下Xcode 10右上导航的样子** 

![](https://github.com/Lobster-King/AppArticles/raw/master/WWDC%202019%20Tutorials/xcode10navi.png)

**再看下Xcode 11导航变化**  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xcode11navi.png)

**Library变化**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xocde11library.png)

**新增Editor Options和Add Editor、以及MiniMap**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/minimap.png)

**其他重要能力提升**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/codecompletion.png)

**主要的变更总结如下：**

* Library（代码块Snippets和图片资源预览），由花括号改成了“+”号。原先Snippets、图片预览两项功能扩展为4项：1.SwiftUI控件库。2.SwiftUI Modifiers。3. Snippets代码块。4.图片资源预览（包括后面提到的SPM中的图片资源）。5.Color选择器。

* Version Editor中把Authors（以作者为视角查看代码更改记录）、Logs（以提交时间为视角查看代码更改记录）移出（集成到Editor Options中），只保留Code Review（Comparison代码比对）功能。

* 新增Editor Options和Add Editor功能区。其中Editor Options包含：展示/隐藏SwiftUI画布、展示/隐藏Authors视角、MiniMap等。Add Editor可以新增多个编辑窗口，终于不需要几个编辑窗口来回切换了。

* 新增MiniMap功能。有了代码地图预览功能，查找代码不再繁琐。搜索代码，MiniMap也同步高亮显示。鼠标在MiniMap移动时，也会有方法高亮提示的功能。

* 其他重要功能提升。主要包括：代码补全能力提升、拖拽代码能力提升、拼写检查等。（由于时间和篇幅原因这部分功能笔者暂未进行实践）

**从上面的更新Tips来看，Xcode正在往好的方向发展，之前饱受开发者诟病的沙雕功能也已慢慢被Apple优化。MiniMap、和Add Editor是个亮点，有了这两个重要功能加持，跟其他IDE相比起码在易用性上不落下风。**

#### 二、Swift包管理工具（SPM）

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/swiftpackagemanager.png)

Xcode终于有了自己的包管理工具了！！！喜大普奔，笔者赶紧尝试了一下SPM。

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

**总结：SPM属于Xcode第一个自产自销的包管理工具（是不是可以理直气壮的跟Cocoapods、Carthage说拜拜了？可能需要很长的路要走。。。😅）。SPM的出现具有划时代的意义，体现了Apple致力于完善Swift生态的坚定决心。**



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

在Stash右下角会有Stash功能选择按钮（也可以在左侧右键弹出）。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/stashoption.png)

简要说下三个选项：

* **Apply Stashed Changes**：跟git stash apply作用一样，会把当前Stash Changes栈顶元素应用到当前分支，Stash Changes栈不会移除这个Stash。
* **Export Stashed Changes as Patch File**：将某个Stash Changes导出一个Patch文件。
* **Delete**：跟git stash pop作用一样，将Stash Changes栈顶元素应用到当前分支，Stash Changes栈会移除这个Stash。


**Cherry-Pick**

假设有两个分支A和B，进行合并分支的时候，你并不想把B的所有commit合并到A，而是想把单个commit合并过来，这时候就得用到git cherry-pick命令了。

![](https://github.com/Lobster-King/AppArticles/raw/master/WWDC%202019%20Tutorials/cherry-pick.png)

选择某个commit，然后cherry-pick，弹出框点击cherry-pick就会信生成一个将该commit合并到master分支，并新生成一个commit。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/cherry-pickoption.png)

**总结：Xcode Git GUI工具虽然加入了Stash、Cherry-pick功能，但对于一些业务场景远远不够，但对于基本的开发来讲已经够用了。**

#### 四、StoryBoard设计工具



